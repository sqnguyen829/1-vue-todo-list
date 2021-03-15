<template>
  <div id="app">
    <AddTodo v-on:add-todo="addTodo"/>
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"/>
  </div>
</template>

<script>
import Todos from '../components/Todos'
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
      todos: []
    }
  },
  methods: {
    deleteTodo(id) {
      console.log(id)
      axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
      .then(() => this.todos = this.todos.filter(todo => todo.id !== id))
      .catch(err => console.log(err))

      // fetch(`https://jsonplaceholder.typicode.com/todos/${id}`,{
      //   method:'DELETE',
      //   headers:{
      //     'Content-Type': 'application/json'
      //   }
      // })
      // .then(() => this.todos = this.todos.filter(todo => todo.id !== id))
      // .catch(err => console.log(err))
    },
    addTodo(newTodo) {
      const { title, completed} = newTodo; //simple destructuring of the object

      axios.post("https://jsonplaceholder.typicode.com/todos", {
        title,
        completed
      })
      .then(res => this.todos = [...this.todos, res.data])
      .catch(err => console.log(err));

      // fetch("https://jsonplaceholder.typicode.com/todos", {
      //   method:'POST',
      //   headers:{
      //     'Content-Type': 'application/json'
      //   },
      //   body: JSON.stringify({
      //     title,
      //     completed
      //   })
      // })
      // .then(res => res.json())
      // .then(data => this.todos = [...this.todos, data])
      // .catch(err => console.log(err))
    }
  },
  created() {
    //created() acts like a componentDidMount
    //https://jsonplaceholder.typicode.com/todos
    axios.get("https://jsonplaceholder.typicode.com/todos?_limit=10") //we'll get a promise from this
    .then(res => this.todos = res.data)
    .catch(err => console.log(err))

    //using axios the compled time is 250ms while using fetch is 127ms
    // fetch("https://jsonplaceholder.typicode.com/todos?_limit=10")
    // .then(res => res.json())
    // .then(data => this.todos = data)
    // .catch(err => console.log(err))
  }
}

</script>

<style>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body {
    font-family: Arial, Helvetica, sans-serif;
    line-height: 1.4;
  }

  .btn {
    display: inline-block;
    border: none;
    background: #555;
    color: #fff;
    padding: 7px 20px;
    cursor: pointer;
  }

  .btn:hover {
    background: #666;
  }
</style>
