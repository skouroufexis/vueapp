<template>
  <div class= 'Task container' v-if='!isRemoved' >
    
    <div class='row taskContainer' >
        <div v-if='!isRemoved' class='col-10'>
            <input v-on:keyup='captureTitle'  v-on:keydown.enter='setTask' class='col-12 input_task' v-bind:value="title" placeholder='Enter your new task here' v-if='!isSet'>
        </div>

        <div class='col-1 action' v-if='isBlank &&!isRemoved &&!isSet' v-bind:title='remove' v-on:click='removeTask'>
            <i class="far fa-trash-alt"></i>
        </div>    
        
        

        <div class='col-10 div_task_name' v-if='isSet' v-on:click='edit' v-bind:class="{ completed: completed  }">
            {{title}}
        </div> 

        <div class='col-1 action' v-if='isSet' v-bind:title='checked' v-on:click='check' v-bind:class="{ clicked: clicked }">
            <i class="fas fa-check"></i>
        </div>    

        <div class='col-1 action' v-if='isSet' v-bind:title='remove' v-on:click='removeTask'>
            <i class="far fa-trash-alt"></i>
        </div>    

        <button class='button_set' v-if="!isBlank" v-on:click='setTask'>
            SET 
        </button>
    </div>
  
  </div>
</template>

<script>


export default {
  name: 'Task',

  mounted:function(){
      let taskInputs=document.getElementsByClassName('input_task');
      let n=taskInputs.length;
      if(n>0)
      {
          taskInputs[n-1].focus();
      }  
      
  }
  ,

  data:function(){return {
      isRemoved:false,
      isBlank:true,
      isSet:false,
      title:'',
      checked:'completed',
      remove:'remove task',
      completed:false,
      clicked:false

    }
  
  },

  props:['taskCounter'],

  
  methods:{
      captureTitle:function(){
          
          let v=event.target.value;
          if(v.trim()!='')
            {

                this.isBlank=false;      
                this.title=v;
            }
            else
            {
                this.isBlank=true;
                this.title='';      
            }
          },
    setTask:function(){
        
        this.isSet=true;
        this.isBlank=true;


        
    },
    edit:function(){
        this.isSet=false;
        this.isBlank=false;
        this.completed=false;
    },

    check:function(){
        this.completed=!this.completed;
        this.clicked=!this.clicked;
    },

    removeTask:function(){
        
        this.isRemoved=true;
    }
  }
}
</script>

<style>
@import 'Task.css';

</style>