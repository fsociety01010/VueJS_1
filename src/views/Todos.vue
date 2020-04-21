<template>
  <div>
    <h2>List todos</h2>
    <router-link to="/">Home</router-link>
    <addTodo 
        @add-todo="addTodo" />
    <hr>
    <select v-model="filter">
      <option value="completed">Completed</option>
      <option value="all">All</option>
      <option value="no-completed">Not completed</option>
    </select>
    <Loader 
        v-if="loading"
    />
    <todolist
        v-else-if="filteredTodos.length"
        v-bind:todos="filteredTodos" 
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
      loading: true,
      filter: 'all'
    };
  },
  components: {
    todolist,
    addTodo,
    Loader
  },
  computed: {
    filteredTodos(){
      if(this.filter === 'all'){
        return this.todos
      } 
      if(this.filter === 'completed'){
        return this.todos.filter( t => t.completed) 
      } 
      if(this.filter === 'no-completed'){
        return this.todos.filter( t => !t.completed) 
      } 
    }
  },
  mounted() {
    fetch("https://jsonplaceholder.typicode.com/todos?_limit=10")
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