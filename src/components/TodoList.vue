<script setup>
import { ref, watch } from 'vue';

import TodoItem from './TodoItem.vue';

const { todoList, deleteTodo, changeStatusTodo, sortTodos, editTodo } = defineProps({
    todoList: Array,
    deleteTodo: Function,
    changeStatusTodo: Function,
    sortTodos: Function,
    editTodo: Function
});

const sortActiveOption = ref('');

watch(sortActiveOption, (newSortActiveOption) => {
    sortTodos(newSortActiveOption)
});
</script>

<template>
    <div v-if="todoList.length > 0" class="todo-list__container">
        <select v-model="sortActiveOption">
            <option value="" disabled selected>Sorting</option>
            <option value="low">From low</option>
            <option value="high">From high</option>
        </select>
        <ul class="todo-list">
            <TodoItem 
                v-for="item in todoList" 
                :key="item.id" 
                :todo="item" 
                :deleteTodo
                :changeStatusTodo 
                :editTodo
            />
        </ul>
    </div>
    <h2 v-else>No tasks</h2>
</template>

<style>
    h2 {
        text-align: center;
    }
    .todo-list {
        width: 100%;
    }
    .todo-list__container {
        display: flex;
        flex-direction: column;
        align-items: end;
    }
    select {
        margin-bottom: 16px;
    }
</style>