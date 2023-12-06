<template>
    <div class="todo-app">
        <h1>Task <span>List</span></h1>

        <TodoForm @addTodo="addTodo" />

        <div class="add-todo-container">
            <h2 v-if="todos.length > 0" class="task-header">My Tasks</h2>
            <p v-else class="no-task">No task yet</p>
            <div v-if="todos.length > 0" class="filter-container">
                <label for="filter-status">Filter by status:</label>
                <select id="filter-status" v-model="filterStatus" class="filter-select">
                    <option value="all">All</option>
                    <option value="done">Done</option>
                    <option value="undone">Undone</option>
                </select>
            </div>

            <div v-if="todos.length > 0" class="filter-container">
                <input id="filter-text" type="text" v-model="filterText" class="filter-text" placeholder="filter by text" />
            </div>
            <TodoList :todos="filteredTodos" @deleteTodo="deleteTodo" @editTodo="editTodo"
                            @toggleTodoStatus="toggleTodoStatus" />
        </div>

        <button v-if="todos.length > 0" @click="clearAllTodos" class="clear-button">
            <i class="fas fa-trash-alt"></i> Clear All
        </button>
        <button v-if="todos.length > 0" @click="clearCompletedTodos" class="clear-button">
            <i class="fas fa-check-circle"></i> Clear Completed
        </button>
    </div>
</template>

<script>
import TodoForm from './TodoForm.vue';
import TodoList from './TodoList.vue';

export default {
    components: {
        TodoForm,
        TodoList,
    },
    data() {
        return {
            filterStatus: 'all',
            filterText: '',
            todos: [],
        };
    },
    computed: {
        filteredTodos() {
            return this.todos.filter((todo) => {
                const isFilteredByStatus =
                    this.filterStatus === 'all' || this.filterStatus === todo.done;
                const isFilteredByText =
                    !this.filterText || todo.text.includes(this.filterText);
                return isFilteredByStatus && isFilteredByText;
            });
        },
    },
    methods: {
        addTodo(newTodo) {
            if (!newTodo.trim()) return;
            this.todos.push({
                id: Date.now(),
                text: newTodo.trim(),
                done: false,
            });
        },
        deleteTodo(index) {
            this.todos.splice(index, 1);
        },
        editTodo({ index, newText }) {
            this.todos[index].text = newText;
        },
        toggleTodoStatus(index) {
            this.todos[index].done = !this.todos[index].done;
        },
        clearAllTodos() {
            this.todos = [];
        },
        clearCompletedTodos() {
            this.todos = this.todos.filter((todo) => !todo.done);
        },
    },
};
</script>

<style scoped></style>
