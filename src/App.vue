<template>
  <div class="container">
    <Header @toggle-add-task="toggleAddTask" title="Task Tracker" :showAddTask="showAddTask"/>
    <div v-if="showAddTask">
      <AddTasks
        @add-task="addTask"
      />
    </div>
    <Tasks 
      @toggle-reminder="toggleReminder" 
      @delete-task="deleteTask" 
      :tasks='tasks'
    />
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Tasks from './components/Tasks.vue'
import AddTasks from './components/AddTask.vue'

export default {
  name: 'App',
  components: {
    Header,
    Tasks, 
    AddTasks
  },
  data(){
    return{
      tasks: [],
      showAddTask: false
    }
  },
  methods:{
      //Append new task to end of list of tasks
    addTask(task){
      this.tasks = [...this.tasks, task]
    },
    deleteTask(id)
    {
      if (confirm(`Are you sure you want to delete ${id}?`)){
        this.tasks = this.tasks.filter((task) => task.id !== id)
      }
    },
    toggleReminder(id){
      //the squiggly brackets in the ternary operator is returning all the tasks with the 
      //same attributes except the reminder one.
      this.tasks = this.tasks.map((task) => task.id === id ? {...task, reminder:!task.reminder} : task)
    }, 
    toggleAddTask(){
      this.showAddTask = !this.showAddTask
    }
  },
  //Simulate data coming in via external https requests and apis using this vue lifecycle
  created(){
    this.tasks = [
      {
        id: 1, 
        text: "Doctor's Appointment",
        day: 'March 1st at 2:30pm',
        reminder: true,
      },
            {
        id: 2, 
        text: 'Meeting at School',
        day: 'March 3st at 1:30pm',
        reminder: true,
      },      {
        id: 3, 
        text: 'Food Shopping',
        day: 'March 3rd at 11:00am',
        reminder: false,
      },
    ]
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: 'Poppins', sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>