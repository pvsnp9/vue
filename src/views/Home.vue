<template>
  <div id="app">
    <AddTodo v-on:add-todo="addTodo"/>
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"/>
  </div>
</template>

<script>
import Todos from '../components/Todos';
import AddTodo from '../components/AddTodo'
import axios from 'axios'

export default {
  name: 'Home',
  components: {
    Todos,
    AddTodo
  },
  data(){
    return {
      todos: [
        
      ]
    }
  },
  methods: {
    deleteTodo(id){
      this.todos = this.todos.filter(todo => todo.id != id)
      axios.delete(`http://jsonplaceholder.typicode.com/todos/${id}`)
        .then((res) => {
          this.todos = this.todos.filter(todo => todo.id != id)
          console.log(res)
          })
        .catch(err => console.log(err))
    },
    addTodo(todo){
      const {title, completed} = todo;
      axios.post('http://jsonplaceholder.typicode.com/todos',{title, completed})
        .then(res => this.todos = [...this.todos, res.data])
        .catch(err => console.log(err))
    }
  },
  created(){
    axios.get('http://jsonplaceholder.typicode.com/todos?_limit=5')
      .then((res) => {
        this.todos = res.data
        })
      .catch((err) => {
        console.log(err)
      })
  }
}
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    /* -webkit-font-smoothing: antialiased; */
    /* -moz-osx-font-smoothing: grayscale; */
    /* text-align: center; */
    /* color: #2c3e50; */
    /* margin-top: 60px; */
  }

  body{
    margin: 0em;
    line-height: 1.4;
  }

  .btn {
    display: inline-block;
    border: none;
    background: #555;
    color: snow;
    padding: 7px 20px;
    cursor: pointer;
  }
  .btn a {
    background: #666;
  }


</style>
