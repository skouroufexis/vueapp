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
      isSet:this.set,
      editing: false,
      title:this.task,
      counter:this.taskCounter,
      checked:'completed',
      remove:'remove task',
      completed:(()=>{
          if(this.isCompleted)
            {
                return this.isCompleted;
            }
        else
            {
                return false;
            }
      })()
      
                ,
      clicked:false,

       
    }
  
  },

  props:['taskCounter','task','set','isCompleted'],

  
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
    //adds the task to the list in the page          
    setTask:function(){
        
        this.isSet=true;
        this.isBlank=true;
        let tasks;
        let task;
        let tasksArray;

        task=this.title+'isCompleted'+this.completed;

        if(localStorage.getItem('tasks')) //tasks already exist in localStorage
        {
            tasks=localStorage.getItem('tasks');
            task=this.title+'isCompleted'+this.completed;
            
            //splitting tasks
            tasksArray=tasks.split(',');
            //controls if new task is added or if existing one is modified
            if(this.editing==true)
                { 

                    let newTasks='';

                    //replace array item with edited task
                    let replaceIndex=(this.counter)-1;
                    tasksArray[replaceIndex]=task;

                    let c;             
                    for(c=0;c<tasksArray.length;c++)
                    {
                       if(c==0)
                        {
                            newTasks=newTasks+tasksArray[c];
                        }
                        else
                        {
                            newTasks=newTasks+','+tasksArray[c];
                        }
                        
                    }
                    
                    localStorage.setItem('tasks',newTasks);
                    
                    this.editing=false;
                }
            else
            {
                tasks=tasks+','+task;
                localStorage.setItem('tasks',tasks);
            }
            
        }
        else
        {            
            
            localStorage.setItem('tasks',task);
        }

    console.log(localStorage.getItem('tasks'));
    },


    //edits task
    edit:function(){
        this.isSet=false;
        this.isBlank=false;
        this.completed=false;
        this.editing=true;
    },

    check:function(){
        
        this.completed=!this.completed;
        this.clicked=!this.clicked;
        
        //change the 'isCompleted' part in the localStorage
        let tasks=localStorage.getItem('tasks');
        
        let tasksArray=tasks.split(',');
        
        let task=this.title+'isCompleted'+this.completed;
        
        
        
    
        //replace array item with edited task
        let replaceIndex=((this.counter)-1);
        tasksArray[replaceIndex]=task;     

        
        let c;             
        let newTasks='';
        for(c=0;c<tasksArray.length;c++)
        {
            if(c==0)
            {
                newTasks=newTasks+tasksArray[c];
            }
            else
            {
                newTasks=newTasks+','+tasksArray[c];
            }
            
            
        }

        
        localStorage.setItem('tasks',newTasks);
        
    },

    removeTask:function(){
        this.isRemoved=true;
        this.counter--;
        

        this.$emit('updateCounter')

        let tasks= localStorage.getItem('tasks');
        let tasksArray=tasks.split(',');


        let removeIndex=(this.counter); 
        
        tasksArray.splice(removeIndex,1);
        //re-populate local storage with the updated number of tasks        
        let counter;
        let newTasks='';
        for (counter=0;counter<tasksArray.length;counter++)
            {
            
                if(counter==0)
                    {
                        newTasks=newTasks+tasksArray[counter];
                    }
                else
                    {
                        newTasks=newTasks+','+tasksArray[counter];
                    }
            
            }
        
        localStorage.setItem('tasks',newTasks);

        let itemsLeft=document.getElementsByClassName('task').length;
        
        
        // clear localStorage if all the tasks are deleted
        if(itemsLeft==1)
        {
            
            localStorage.clear();
        }
         
     
    }
  }
}
</script>

<style>
@import 'Task.css';

</style>