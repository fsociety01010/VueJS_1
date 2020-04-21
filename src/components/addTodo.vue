<template>
  <form @submit.prevent="onSubmit">
    <input type="text" v-model="title" />
    <button type="submit">Add item</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      title: ""
    };
  },
  methods: {
    onSubmit() {
      console.log("submit", this.title);
      if (this.title.trim()) {
          const newTodo = {
              id: Date.now(),
              title: this.title,
              completed: false
          }
          this.$emit("add-todo", newTodo)
          this.title = ''
      }
    }
  }
};
</script>

<style  scoped>
form {
  /* display: flex; */
  margin: 1rem;
}
input {
  width: 400px;
}
</style>