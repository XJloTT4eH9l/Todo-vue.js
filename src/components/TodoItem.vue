<script setup>
    const { todo, deleteTodo, changeStatusTodo, editTodo } = defineProps({ 
        todo: Object ,
        deleteTodo: Function,
        changeStatusTodo: Function,
        showHidePopup: Function,
        editTodo: Function
    });

    const onTaskChange = (event, id) => {
        const value = event.target.value;
        editTodo(id, value);
    }
</script>

<template>
    <li :class="['todo-item', { 'active': todo.completed }]">
        <div class="todo-item__task">
            <input 
                type="checkbox" 
                :checked="todo.completed"
                @change="changeStatusTodo(todo.id)"
            >
            <input
                type="text"
                class="todo-item__text"
                :value="todo.task"
                @change="onTaskChange($event, todo.id)"
            >
        </div>
        <button class="btn delete" @click="deleteTodo(todo.id)">Delete</button>
        <div :class="['priority', todo.priority]">{{ todo.priority }}</div>
    </li>
</template>

<style>
    .todo-item {
        position: relative;
        width: 100%;
        padding: 30px;
        border: 2px solid black;
        border-radius: 6px;
    }
    .todo-item.active .todo-item__task {
        text-decoration: line-through;
    }
    .todo-item__task {
        display: flex;
        align-items: center;
        gap: 8px;
    }
    .todo-item__text {
        outline: none;
        border: none;
        background-color: transparent;
    }
    .todo-item:not(:last-child) {
        margin-bottom: 8px;
    }
    .priority {
        position: absolute;
        right: 10px;
        top: 10px;
        width: 70px;
        text-align: center;
        padding: 2px 10px;
        background-color: transparent;
        border: 2px solid black;
        border-radius: 6px;
    }
    .delete {
        margin-right: 10px;
    }
</style>