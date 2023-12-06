<template>
  <li class="todo-item">
    <input type="checkbox" v-model="todo.done" class="todo-checkbox" />
    <p :class="{ done: todo.done }">{{ todo.text }}</p>
    <button @click="editTodo" class="edit-button">
      <i class="fas fa-edit"></i> Edit
    </button>
    <button @click="deleteTodo" class="delete-button">
      <i class="fas fa-trash-alt"></i> Delete
    </button>
  </li>
</template>

<script>
export default {
  props: ['todo', 'index'],
  methods: {
    deleteTodo() {
      this.$emit('deleteTodo', this.index);
    },
    editTodo() {
      const newText = prompt('Enter new todo text:', this.todo.text);
      if (newText) {
        this.$emit('editTodo', { index: this.index, newText });
      }
    },
  },
};
</script>

<style >
</style>
