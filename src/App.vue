<template>
  <div class="container">
    <Header @toggle-add-task="toggleAddTask()" title="Task Tracker" 
    :showAddTask="showAddTask "/>   
    <div v-show="showAddTask" >
        <AddTask @add-task="addTask" />
    </div>
    <Tasks  @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks" />
  </div>
</template>

<script>
// @ is an alias to /src
import Header from './components/Header'
import Tasks from './components/Tasks'
import AddTask from './components/AddTask'

export default {

  name: 'App',
  components: {
    Header,
    Tasks,
    AddTask
  },

  data(){
    return{
      tasks:[],
      showAddTask: false
    }
  },
  methods:{
    // ADD TASK TO NODE API
    async addTask(task){

        const res = await fetch('/api/tasks/add', {
          method:'POST',
          headers: {
            'Content-Type':'application/json',           
          },
          body: JSON.stringify(task)
          })

          const data = res.json()

          return data

        this.tasks = [...this.tasks, data]
    },

    // toggle add task
    toggleAddTask(){
      this.showAddTask = !this.showAddTask
    },

    // DELETE TASK
    async deleteTask(id){
      if (confirm('Are you sure?')){
         const res = await fetch(`/api/${id}`, {method: 'DELETE'})
         res.status == 200 ? (this.tasks = this.tasks.filter((task) => task.id !== id)) : alert('Error Deleting Task')
          
      }
    },

    toggleReminder(id){
      this.tasks = this.tasks.map((task) => task._id === id ? 
      {...task, reminder : !task.reminder} : task) 
    },

    // FETCH TASKS FROM NODE API
    async fetchTasks(){
      const res = await fetch('api/tasks')
      
      const data = await res.json()

      return data
    },

    async fetchTask(id){
      const res = await fetch(`api/tasks/${_id}`)
      
      const data = await res.json()

      return data
    }

  },
  async created(){
    this.tasks = await this.fetchTasks()
  }
  ///////////////////////////////////////////////////////////////////// 
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