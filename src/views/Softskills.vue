<template>
 <div id="cards">
    
    <Status class="StatusShow" v-bind:img="status" v-show="loading" />

    <section id="" class="post" v-for="item in info" :key="item.id" style="float: left;">


        <div class="content"  >
      <br/>
  
            <div class="flip-card">
            <div class="flip-card-inner">
                <div class="flip-card-front">
                    <div class="head-and-text">
                    <div v-bind:class="item.fields.css + ' card-content'">
                        <div style="text-align:right;padding-right:10px;padding-top:5px;font-size:1.5em;">                   
                            {{ item.fields.SubjectName[0] }}
                        </div>
                        <div style="text-align:left;padding-top:20px;padding-left:10px;font-size:2em;line-height: 85%;">                    
                           {{ item.fields.Title_en_us }}
                        </div>
                    </div>
                    
                    <img v-if="item.fields.Icon" v-bind:src="item.fields.Icon[0].url" v-bind:alt="item.fields.Title_en_us" style="width:100%; max-height:100px;" />
                    <vue-markdown class="front-text">{{ item.fields.Description_en_us }}</vue-markdown>
                    
                    </div>
                    <div  style="text-align:left;padding-left:10px;font-size:1em;"> 
                       
                    </div>
                </div>

                <div class="flip-card-back">
                  
                <h1>{{ item.fields.Title_en_us }}</h1>
     
                <p><vue-markdown>{{ item.fields.Description_en_us }}</vue-markdown></p>

                </div>

            </div>
            </div>

        </div>

    </section>
    <section class="">

    <div id="pages" style="text-align:center" v-show="ready">
        <p><button  :disabled='backDisabled' v-on:click="backward">previous page</button> | <button  :disabled='forwardDisabled' v-on:click="forward">next page</button></p>
    </div>
    </section>
</div>
</template>


<style >
@import url('https://fonts.googleapis.com/css2?family=Goldman&display=swap');
/* The flip card container - set the width and height to whatever you want. We have added the border property to demonstrate that the flip itself goes out of the box on hover (remove perspective if you don't want the 3D effect */
.flip-card {

margin: 10px;
  width: 600px;
  max-width: 100%;
  height: 700px;
  font-size: .7em;
  border-radius: 0 0 15px 15px;
  
  perspective: 1000px; /* Remove this if you don't want the 3D effect */
}


.card-content{
    border-radius: 15px 15px 0 0;
    height: 100px;
    
    padding-top: 3px;;
    font-family: 'Goldman', cursive;
}
.head-and-text{
height: 410px;
}

.craft{

    background-color:#8e4e9d;
}
.tech-trend{
    background-color:#f39322;
}
.circular{
    background-color:#95b535;
}
.wild{
    background-color:#445269;
}
.front-text{
padding: 1em;
}

/* This container is needed to position the front and back side */
.flip-card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  text-align: center;
  transition: transform 0.8s;
  transform-style: preserve-3d;
  
}

/* Do an horizontal flip when you move the mouse over the flip box container */
.flip-card:hover .flip-card-inner {
  transform: rotateY(180deg);
}

/* Position the front and back side */
.flip-card-front, .flip-card-back {
    border-radius: 25px;
  position: absolute;
  width: 100%;
  height: 100%;
  
  -webkit-backface-visibility: hidden; /* Safari */
  backface-visibility: hidden;
}

/* Style the front side (fallback if image is missing) */
.flip-card-front {
  background-color:ivory;
  color: black;
  background-color: rgb(212, 209, 209);
}

/* Style the back side */
.flip-card-back {
  background-color: rgb(212, 209, 209);
  color: white;
  transform: rotateY(180deg);
}
</style>

<script>
import axios from 'axios';
import Status from '@/components/Status.vue'
import VueMarkdown from 'vue-markdown'

async function getData(viewStatus)
{

    const config = 
    {
        headers: 
        {
        
        
        }
    }
var settings = {
    'table': viewStatus.table,
    'view': viewStatus.view,
    'pageSize': viewStatus.pageSize,
    'offset': viewStatus.offset
}

const formData = new FormData();
formData.append('data', JSON.stringify(settings));
formData.append('action', 'view');

viewStatus.loading = true;
await axios.post (viewStatus.$baseUrl, formData, config)
        .then(response => 
        {
            //alert('hi');
            viewStatus.info = response.data.records;
            if(response.data.offset)
            {
                viewStatus.offset = response.data.offset;
                viewStatus.ready = true;
                // alert(response.data.offset);
            }
            else
            {
                viewStatus.offset = "";
                
            }
            viewStatus.loading = false;

        }).catch
        (
            function (error) 
            {
                console.log(error)
                //viewStatus.status = error;
                viewStatus.loading = false;
            }
        )
}

export default 
{
    name: "softskills",
    components: {Status, VueMarkdown},

    data() 
    {
    return {
        info: null,
        ready: false,
        loading: false,
        status: "",
        offset: "",
        offsetHistoryCursor: 0,
        pageSize: 30,
        offsetHistory: [],  
        table: "Competences",
        view: "Public"
        }
    },
    methods:
    {
        forward: function() 
        {
            this.offsetHistoryCursor++;
            this.offsetHistory[this.offsetHistoryCursor] = this.offset;
            getData(this);
           
        },
        backward: function()
        {
            this.offsetHistoryCursor--;
            if(this.offsetHistoryCursor < 0) this.offsetHistoryCursor =0;
            this.offset = this.offsetHistory[this.offsetHistoryCursor];
            getData(this);
        }
    },
    computed: {
  	    backDisabled: function(){
              if(this.offsetHistoryCursor == 0)
                return true;
                else
    	        return false;
        },
        forwardDisabled: function(){
           // if(this.offset)
    	    return  (this.offsetHistoryCursor >0 || this.offsetHistoryCursor ==0) && this.offset == "";
        }  
  },
    created() 
    {
        this.offsetHistory[0]="";
        getData(this);
    },
}


</script>


