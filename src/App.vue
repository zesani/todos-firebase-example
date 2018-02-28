<template>
  <div id="app">
    <!-- firebase -->
    <input type="text" v-model="text">
    <button @click="addTodoFire"> Add </button><br>
    <ul>
      <li v-for="todo in todosFire" :key="todo['.key']">
        <input type="text" :value="todo.text" @input="updateTodoFire(todo['.key'], $event.target.value)">
        <button @click="removeTodoFire(todo['.key'])">X</button>
      </li>
    </ul>
    <!-- local -->
    <input type="text" v-model="text">
    <button @click="addTodo"> Add </button><br>
    <ul>
      <li v-for="todo in todos" :key="todo.key">
        <input type="text" :value="todo.text" @input="updateTodo(todo.key, $event.target.value)">
        <button @click="removeTodo(todo.key)">X</button>
      </li>
    </ul>
  </div>
</template>

<script>
import firebase from 'firebase'
var config = {
  apiKey: 'AIzaSyCqBx_7KsT8u7fHV8kv2zPW_-hqC1uVHuI',
  authDomain: 'todos-firebase2.firebaseapp.com',
  databaseURL: 'https://todos-firebase2.firebaseio.com',
  projectId: 'todos-firebase2',
  storageBucket: 'todos-firebase2.appspot.com',
  messagingSenderId: '941427621428'
}
var firebaseApp = firebase.initializeApp(config)
var db = firebaseApp.database()
var todosRef = db.ref('todos')
export default {
  name: 'App',
  firebase: {
    todosFire: todosRef
  },
  data () {
    return {
      text: '',
      todos: []
    }
  },
  methods: {
    addTodoFire () {
      todosRef.push({
        text: this.text
      })
    },
    updateTodoFire (key, text) {
      // todosRef.child(key).child('text').set(text)
      db.ref('todos/' + key).set({
        text: text
      })
    },
    removeTodoFire (key) {
      // todosRef.child(key).remove()
      db.ref('todos/' + key).remove()
    },
    addTodo () {
      this.todos.push({
        text: this.text,
        key: Date.now() + Math.floor((Math.random() * 1000000) + 10000)
      })
      this.text = ''
    },
    updateTodo (key, text) {
      // const index = this.todos.findIndex((todo) => todo.key === key)
      // if (index > -1) {
      //   this.todos[index].text = text
      // }
      const todo = this.todos.find((todo) => todo.key === key)
      if (todo) {
        todo.text = text
      }
    },
    removeTodo (key) {
      // const index = this.todos.findIndex(function (todo) {
      //   return todo.key === key
      // })
      const index = this.todos.findIndex((todo) => todo.key === key)
      if (index > -1) {
        this.todos.splice(index, 1)
      }
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
