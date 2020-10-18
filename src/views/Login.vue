<template>
 <div id="login">
    <section id="banner" class="banner">
<!--<Error class="" v-bind:img="status" v-show="errors.length>0" />-->

 
        <div class="content" v-if="user && user.Token && !signUp">
            <h1>Profile</h1>
            <p><input type="text" disabled name="loggedInName" id="loggedInUserName" value="" placeholder="Name" autocomplete="off" v-model="user.Username"></p>
            <p><input type="text" name="loggedInName" id="loggedInName" value="" placeholder="First name" autocomplete="off"  v-model="user.Name"></p>
            <p><input type="text" name="loggedInLastName" id="loggedInLastName" value="" placeholder="Last name" v-model="user.LastName"></p>
            <p><input type="email" name="Email" id="emailProfile" value="" placeholder="E-mail" v-model="user.Email"></p>
            <p><input type="url" name="AvatarUrl" id="avatarUrl" value="" placeholder="Avatar Url" v-model="user.AvatarUrl" ></p>
            <ul class="actions">
                <li><a href="#" class="button primary" v-on:click="updateProfile()">Update</a></li>
                <li><a href="#" class="button" v-on:click="logOut()">Log out</a></li>
                <li><a href="#" class="button" v-on:click="deleteProfile()">Delete profile</a></li>
            </ul>
        </div>

        <div class="content" v-if="!user || !user.Token && !signUp">

                <h1>Login</h1>
                <p><input type="text" name="loginName" id="loginName" value="" placeholder="Username" v-model="user.Username"></p>
                <p><input type="password" name="Password" id="Password" value="" placeholder="Password" v-model="user.Password"></p>
                <ul class="actions">
                    <li><a href="#" class="button primary" v-on:click="getLogin()">Login</a></li>
                    <li><a href="#" class="button" v-on:click="applyForAccount()">Sign up</a></li>
                </ul>
        </div>

        <div class="content" v-if="signUp">

                <h1>Sign up</h1>
                <p><input type="text" name="loginName" id="loginName" value="" placeholder="Username" v-model="user.Username"></p>
                <p><input type="text" name="loggedInName" id="loggedInName" value="" placeholder="First name" v-model="user.Name"></p>
                <p><input type="text" name="loggedInLastName" id="loggedInLastName" value="" placeholder="Last name" v-model="user.LastName"></p>
                
                <p><input type="password" name="Password" id="Password" value="" placeholder="Password" v-model="user.Password"></p>
                <p><input type="email" name="Email" id="email" value="" placeholder="E-mail" v-model="user.Email"></p>
                <ul class="actions">
                    <li><a href="#" class="button primary" v-on:click="getAccount()">Sign up</a></li>
                    <li><a href="#" class="button" v-on:click="signUp=false">Login</a></li>
                </ul>
                
        </div>

        <span class="image object" v-if="errors.length ==0">
            <img v-bind:src="localAvatarUrl" v-bind:alt="user.Name" />
        </span>
        <span  class="image object" v-show="errors.length>0">
            <ul id="err" class="" v-for="item in errors" :key="item.index" style="color:red;margin: 0 0 0 0">
                <li>{{ item }}</li>
            </ul>   
        </span>
    </section>
<Status class="StatusShow" v-bind:img="status" v-show="loading" />        

</div>

</template>


<style >

</style>

<script>
import axios from 'axios';
import Status from '@/components/Status.vue'


async function getData(viewStatus,action)
{

    viewStatus.loading = true;
    viewStatus.errors = [];
    const config = 
    {
        headers: 
        {
        }
    }

    const formData = new FormData();
    formData.append('action', action);
    formData.append('data', JSON.stringify(viewStatus.user));

    //alert("action:"+action+":userin:"+JSON.stringify(viewStatus.user));

    await axios.post (viewStatus.$baseUrl, formData, config)
            .then(response => 
            {
                //alert(baseUrl);
                //alert('response:'+JSON.stringify(response))
                if(response.data.errors)
                {
                    for(var i=0; i<response.data.errors.length;i++)
                    {
                        viewStatus.errors.push(response.data.errors[i]);
                    }
                    viewStatus.loading = false;
                    return;
                }
                if(action == 'delete')
                {
                    viewStatus.clearUser();
                    viewStatus.loading = false;
                    return;
                }
                //multiple record return
                var userFound = false;
                if(action == "read" && response.data.records[0])
                {
                    viewStatus.user = response.data.records[0].fields;
                    userFound = true;
                }
                if(action == "read" && !userFound)
                {
                    //login failed
                    viewStatus.errors.push("Login failed");
                }

                //single record return
                if((action == "create" || action == "update") && response.data.fields)
                {
                    viewStatus.user = response.data.fields;  
                    userFound = true;
                } 

                if(userFound)
                {
                    //store in browser
                    sessionStorage.user = JSON.stringify(viewStatus.user)
                    localStorage.user = JSON.stringify(viewStatus.user)
                    //update login/profile label
                    if(viewStatus.user.Name) document.getElementById('un').innerHTML = viewStatus.user.Name;
                    viewStatus.signUp = false;
                    if(viewStatus.user.AvatarUrl && viewStatus.user.AvatarUrl != "")
                    {
                        viewStatus.localAvatarUrl = viewStatus.user.AvatarUrl;
                    }
                }
                else
                {
                    //alert(response.data);
                    viewStatus.errors.push("User not found");
                    document.getElementById('un').innerHTML = "Login";
                }

                viewStatus.loading = false;

            }).catch
            (
                function (error) 
                {
                    console.log(error);
                    //alert(error);
                    viewStatus.errors.push("An error occored");
                    viewStatus.loading = false;
                }
            )
}

export default 
{
    name: "login",
    components: {Status},

    data() 
    {
    return {
        data: null,
        status: "",
        signUp: false,
        table: "User",
        loading: false,
        defaultAvatarUrl: "https://drive.google.com/uc?id=1DPL6WFvmihLxo_pDOcZZg-gBHAgzJLZ2",
        localAvatarUrl: "https://drive.google.com/uc?id=1DPL6WFvmihLxo_pDOcZZg-gBHAgzJLZ2",
        user: {},
        errors: []
        }
    },
    methods:
    {
        getLogin: function()
        {
            getData(this, 'read');
        },
        logOut: function()
        {
            this.clearUser();
            
        },
        applyForAccount: function()
        {
            this.signUp = true;
        },
        getAccount: function()
        {
            getData(this, 'create');
        },
        clearUser: function()
        {
            this.user = {};
            localStorage.clear();
            sessionStorage.clear();
            document.getElementById('un').innerHTML = "Log in";
            this.localAvatarUrl = this.defaultAvatarUrl;

        },
        updateProfile: function()
        {
        
            getData(this, 'update');
            
        },
        deleteProfile: function()
        {
            if(confirm("Do you really want to remove your account? This can not be undone!"))
            {
                getData(this, 'delete');
            }
            
        },
        validateInput: function()
        {
            //todo
            //if(this.user.Name)
        }
    },
    computed: {

  },
  mounted()
  {
     
  },
  
    created() 
    {

     	if(localStorage.user)
        {
            this.user = JSON.parse(localStorage.user);
            
            if(this.user.AvatarUrl && this.user.AvatarUrl.length >5) this.localAvatarUrl = this.user.AvatarUrl;
            if(this.user.Name) document.getElementById('un').innerHTML = this.user.Name;
        }
  
    }
}


</script>


