<template>
  <div class="todo-list">
    <div class="card">
      <div v-for="todo in todos" :key="todo.id" class="card-body d-flex justify-content-between">
        <span>{{ todo.title }}</span>
        <span @click="deleteTodo(todo)">Delete</span>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'TodoList',
  props: {
    todos: {
      type: Array,
      required: true,
    }
  },
  computed: {
    options() {
      return this.$store.getters.options
    }
  },
  methods: {
    deleteTodo(todo){
      const SERVER_IP = process.env.VUE_APP_SERVER_IP

      axios.delete(`${SERVER_IP}/api/v1/todos/${todo.id}`, this.options)
        .then(()=>{
          const idx = this.todos.indexOf(todo)
          if (idx > -1){
            this.todos.splice(idx, 1)
          }
        })
        .catch(error=>{
          console.log(error)
        })
    }
  }
}
</script>

<style>

</style>