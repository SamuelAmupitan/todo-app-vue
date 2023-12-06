<template>
  <ul class="todo-list">
    <TodoItem v-for="(todo, index) in todos" :key="todo.id" :todo="todo" :index="index" @deleteTodo="deleteTodo" @editTodo="editTodo" @toggleTodoStatus="toggleTodoStatus" />
  </ul>
</template>

<script>
import TodoItem from './TodoItem.vue';

export default {
  components: {
    TodoItem,
  },
  props: ['todos'],
  methods: {
    deleteTodo(index) {
      this.$emit('deleteTodo', index);
    },
    editTodo({ index, newText }) {
      this.$emit('editTodo', { index, newText });
    },
    toggleTodoStatus(index) {
      this.$emit('toggleTodoStatus', index);
    },
  },
};
</script>

<style scoped>
</style>
