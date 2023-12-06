<template>
  <div class="add-task">
    <input v-model="newTodo" @keyup.enter="addTodo" placeholder="Input task" />
    <button @click="addTodo" class="add-button">
      <i class="fas fa-plus"></i> Add Task
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
    };
  },
  methods: {
    addTodo() {
      this.$emit('addTodo', this.newTodo);
      this.newTodo = '';
    },
  },
};
</script>

<style scoped>
</style>
