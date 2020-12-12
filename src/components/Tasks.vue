<template>
    <div id="Tasks">
      <div class='container'>
          <div class='container'>
              <div class='row'>
                  <button v-on:click='add("",false,false)'>+</button> 
                  <div class='div_button_label'>New Task</div>
              </div>

              <div class='row' v-for='task in tasks' :key="task.taskCounter"> 
                  <Task v-bind:taskCounter='task.counter' 
                        v-bind:task='task.task'
                        v-bind:set='task.set' 
                        v-bind:isCompleted='task.isCompleted'
                        v-on:updateCounter='updateCounter'
                        class="task"
                        
                  />
              </div>
          </div>
      </div> 
  </div>
</template>

<script>



import Task from './Task.vue';

export default {

  mounted(){
    // localStorage.clear();


    let tasks;
    
    //previous tasks exist in memory
    if(localStorage.getItem('tasks'))
    {
      
      //separate tasks
      tasks=localStorage.getItem('tasks');
      tasks=tasks.split(',');
      tasks.forEach(task => {
      //determine if tasks are completed
      let taskCompleted=task.split('isCompleted');
      let taskName=taskCompleted[0];
          if(taskCompleted[1]=='true'){taskCompleted[1]=true;}
          if(taskCompleted[1]=='false'){taskCompleted[1]=false;}

      
      
      this.add(taskName,true,taskCompleted[1]);
    });      
    }
  },

  name: 'Tasks',

  components: {
    Task
  },

  data:function(){return {
      taskCounter:0,
      tasks:[],
    }
  },
  
  methods:{
      add:function(task,set,completed){
         
          this.taskCounter++;          
          this.tasks.push({counter:this.taskCounter,task:task,set:set, isCompleted:completed});
          
      },

      //decrements counter by 1 when a task is deleted
      updateCounter:function(){
        this.taskCounter--;
      }
  } 
}
</script>


<style>
@import 'Tasks.css';


</style>