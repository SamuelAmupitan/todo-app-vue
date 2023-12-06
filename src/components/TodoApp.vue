<template>
    <div class="todo-app">
        <h1>Task <span>List</span></h1>

        <TodoForm @addTodo="addTodo" />

        <div class="add-todo-container">
            <p v-if="todos.length < 1" class="task-header">No task yet</p>
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
            this.saveTodosToLocalStorage();
        },
        deleteTodo(index) {
            this.todos.splice(index, 1);
            this.saveTodosToLocalStorage();
        },
        editTodo({ index, newText }) {
            this.todos[index].text = newText;
            this.saveTodosToLocalStorage();
        },
        toggleTodoStatus(index) {
            this.todos[index].done = !this.todos[index].done;
            this.saveTodosToLocalStorage();
        },
        clearAllTodos() {
            this.todos = [];
            this.saveTodosToLocalStorage();
        },
        clearCompletedTodos() {
            this.todos = this.todos.filter((todo) => !todo.done);
            this.saveTodosToLocalStorage();
        },
        saveTodosToLocalStorage() {
            localStorage.setItem('todos', JSON.stringify(this.todos));
        },
        loadTodosFromLocalStorage() {
            const storedTodos = localStorage.getItem('todos');
            if (storedTodos) {
                this.todos = JSON.parse(storedTodos);
            }
        },
    },
    mounted() {
        // Load todos from localStorage when the component is mounted
        this.loadTodosFromLocalStorage();
    },
};
</script>

<style scoped></style>
