<template>
 <div id="blog">
    
    <Status class="StatusShow" v-bind:img="status" v-show="loading" />

    <section id="" class="post" v-for="item in info" :key="item.id">


        <div class="content" >
        <br/>
        <span class="image object">
                <img v-bind:src="item.fields.Photos[0].url" v-bind:alt="item.fields.Title" style="width:100%" />
        </span>
            <header>
                <h1>{{ item.fields.Title }}</h1>
                <p><vue-markdown>{{ item.fields.Subtitle }}</vue-markdown></p>
                <h5>{{ item.fields.PublishDate }} </h5>
            </header>
            <p><vue-markdown>{{ item.fields.Article }}</vue-markdown></p>
            <ul class="actions">
                <li><a href="#" class="button big">Learn More</a></li>
            </ul>
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
    name: "projects",
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
        pageSize: 5,
        offsetHistory: [],  
        table: "Blog",
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


