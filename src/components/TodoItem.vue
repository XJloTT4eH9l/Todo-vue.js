<script setup lang="ts">
    import type { PropType } from 'vue'
    import { ITodoItem } from '@/types'

    defineProps({ 
        todo: Object as PropType<ITodoItem>
    });
    
    const emit = defineEmits<{
        deleteTodo: [id: number],
        changeStatusTodo: [id: number],
        editTodo: [id: number, value: string]
    }>()
</script>

<template>
    <li :class="['todo-item', { 'active': todo.completed }]">
        <div class="todo-item__task">
            <input 
                type="checkbox"
                class="todo-item__checkbox" 
                :checked="todo.completed"
                @change="$emit('changeStatusTodo', todo.id)"
            >
            <input
                type="text"
                class="todo-item__text"
                :value="todo.task"
                @change="$emit('editTodo', todo.id, ($event.target as HTMLInputElement).value)"
            >
        </div>
        <button class="btn todo-item__delete-btn" @click="$emit('deleteTodo', todo.id)">Delete</button>
        <div :class="['todo-item__priority', todo.priority]">{{ todo.priority }}</div>
    </li>
</template>

<style lang="scss">
    .todo-item {
        position: relative;
        width: 100%;
        padding: 30px;
        border: 2px solid black;
        border-radius: 6px;
        &:not(:last-child) {
            margin-bottom: 8px;
        }
        &.active {
            .todo-item__task {
                text-decoration: line-through;
            }
        }
        &__task {
            display: flex;
            align-items: center;
            gap: 8px;
        }
        &__text {
            outline: none;
            border: none;
            background-color: transparent;
        }
        &__checkbox {
            cursor: pointer;
        }
        &__delete-btn {
            margin-right: 10px;
        }
        &__priority {
            position: absolute;
            right: 10px;
            top: 10px;
            width: 70px;
            text-align: center;
            padding: 2px 10px;
            background-color: transparent;
            border: 2px solid black;
            border-radius: 6px;
            &.high {
                border-color: red;
            }
            &.low {
                border-color: green;
            }
        }
    } 
</style>