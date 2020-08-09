<template>
    <div id="Tasks">
    
    <div class='container'>
        <div class='container'>
             <div class='row'>
            
                <button v-on:click='add'>+</button> <div id='div_button_label'>New Task</div>
                
             </div>

             <div class='row' v-for='task in taskCounter' :key="task">
                
                <Task v-bind:taskCounter='taskCounter' />
                
            </div>

         </div>

    </div>
    
    
    
    
  </div>
</template>

<script>

import Task from './Task.vue';


export default {


  name: 'Tasks',

  components: {
    Task
  },

  data:function(){return {
      taskCounter:0

    }
  },
  
  methods:{
      add:function(){
      
          this.taskCounter++;
          

      }
    

  }

  
  
}
</script>


<style>
@import 'Tasks.css';


</style>