<template>
  <div>
    <h2>List todos</h2>
    <router-link to="/">Home</router-link>
    <addTodo 
        @add-todo="addTodo" />
    <hr>
    <Loader 
        v-if="loading"
    />
    <todolist
        v-else-if="todos.length"
        v-bind:todos="todos" 
        @remove-todo="removeTodo" />
    <p v-else>No todos</p>
  </div>
</template>

<script>
import todolist from "@/components/todoList";
import addTodo from "@/components/addTodo";
import Loader from "@/components/loader";

export default {
  name: "App",
  data() {
    return {
      todos: [],
      loading: true
    };
  },
  components: {
    todolist,
    addTodo,
    Loader
  },
  mounted() {
    fetch("https://jsonplaceholder.typicode.com/todos")
      .then(response => response.json())
      .then(json => {
          setTimeout(() => {
            this.todos = json
            this.loading = false
          }, 1000);
          
      })

  },
  methods: {
    removeTodo(id) {
      this.todos = this.todos.filter(td => td.id !== id);
    },
    addTodo(todo) {
      this.todos.push(todo);
    }
  }
};
</script>