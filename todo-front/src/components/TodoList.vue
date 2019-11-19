<template>
  <div class="todo-list">
    <div class="card">
      <div v-for="todo in todos" :key="todo.id" class="card-body d-flex justify-content-between">
        <span>{{ todo.title }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TodoList',
  props: {
    todos: {
      type: Array,
      required: true,
    }
  }
}
</script>

<style>

</style>