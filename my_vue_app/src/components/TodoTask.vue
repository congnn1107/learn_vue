<template>
  <div class="todo-list">
    <div class="head">
      <h1>Tasks list</h1>
      <input type="text" placeholder="Enter task name" v-model="task.name"> <button class="btn-add" @click="addTask()">Add</button>
    </div>
    <div class="tasks">
      <div :class="{done: task.status == 2}" class="task" v-for="task in tasks" :key="task.id">
        <span>{{task.name}}</span>
        <input type="checkbox" @change="updateTask(task)" v-model="task.status" true-value="2" false-value="1">
        <span><button class="btn-delete" @click="deleteTask(task.id)">Delete</button></span>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "TodoTask",
    data() {
      return {
        tasks: null,
        task: {
          name: "",
          status: 1
        }
      }
    },
    mounted(){
      this.fetchData()
    },  
    methods: {
      fetchData(){
        let url = "http://localhost:3000/tasks?_sort=id&_order=desc"
        fetch(url,{
          method: "GET",
          headers: {
            'Cache-Control': 'no-cache, no-store, must-revalidate',
            'Pragma': 'no-cache',
            'Expires': 0
          }
        })
        .then(res => res.json())
        .then((data) => {
          console.log(data)
          this.tasks = data
        })
      },
      deleteTask(id){
        let url = `http://localhost:3000/tasks/${id}`
        fetch(url,{
          method: "DELETE"
        }).then(res=>{
          if (res.ok){
            this.tasks = this.tasks.filter(item => item.id != id)
            console.log("OK")
          }
          else{
            console.error(res.error)
          }
        })
      },
      addTask(){
        console.log(this.task)
        let url =  "http://localhost:3000/tasks"
        fetch(url,{
          method: "POST",
          headers: {
            'Content-Type': "application/json"
          },
          body: JSON.stringify(this.task)
        }).then(res => {
          if(res.ok){
            return res.json()
          }
        }).then(data => {
          console.log(data)
          this.tasks.unshift(data)
          this.task.name = ""
        })
      },
      updateTask(task){
        let url = `http://localhost:3000/tasks/${task.id}`
        fetch(url,{
          method: 'PATCH',
          headers: {
            'Content-Type' : 'application/json'
          },
          body: JSON.stringify(task)
        }).then(res => {
          if(res.ok){
            return res.json()
          }else{
            console.error(res.error)
          }
        }).then(data => {
          console.log(data)
          task = data
        })
      }
    }
  }
</script>
<style>
  .todo-list {
    min-width: 250px;
  }
  .todo-list h1{
    text-transform: uppercase;
    color: orangered;
    text-align: center;
  }
  .todo-list .tasks {
    margin-top: 30px;
  }
  .task {
    border: 1px solid lightgray;
    border-radius: 3px;
    padding: 2px 3px;
    color: gray
  }
  button {
    border: none;
    outline: none;
    cursor: pointer;
    background: none;
    padding: 3px 5px;
    border-radius: 3px;
    color: #fff;
    float: right;
    opacity: .7;
  }
  button:hover {
    opacity: 1;
  }
  button.btn-add {
    background-color: rgb(106, 194, 34) ;
  }
  button.btn-delete {
    background-color: orangered;
  }
  .task + .task {
    margin-top: 25px
  }
  .done {
    text-decoration: line-through;
    background-color: lightgray;
    color: gray;
  }
  input[type="text"]{
    border: none;
    outline: none;
    width: 80%;
  }
  input[type="checkbox"]{
    margin-left: 1rem;
    padding: 5px;
  }
</style>