<script setup lang="ts">
    import { ref, watch } from 'vue';
    import { ITodoItem } from '@/types';

    import TodoItem from './TodoItem.vue';

    const todoList = defineModel<ITodoItem[]>('todoList');
    const sortActiveOption = ref('');

    const deleteTodo = (id: number) => {
        todoList.value =  todoList.value.filter(todo => todo.id !== id);
    }
    const changeStatusTodo = (id: number) => {
        todoList.value = todoList.value.map(todo => {
            if(todo.id === id) {
                const newStatus = !todo.completed;
                return {...todo, completed: newStatus}
            }
            else {
                return todo
            }
        });
    }
    const editTodo = (id: number, value: string) => {
        todoList.value = todoList.value.map(todo => {
            if(todo.id === id) {
                return {...todo, task: value}
            } else {
                return todo
            }
        });
    }
    const sortTodos = (option :string) => {
        switch(option) {
            case 'high':
                todoList.value = todoList.value.sort((a, b) => a.priority.localeCompare(b.priority));
                break;
            case 'low':
                todoList.value = todoList.value.sort((a, b) => b.priority.localeCompare(a.priority));
                break
            default:
                break;
        }
    }

    watch(sortActiveOption, (newSortActiveOption) => {
        sortTodos(newSortActiveOption)
    });
</script>

<template>
    <div v-if="todoList.length > 0" class="todo-list__container">
        <select class="todo-list__select" v-model="sortActiveOption">
            <option value="" disabled selected>Sorting</option>
            <option value="low">From low</option>
            <option value="high">From high</option>
        </select>
        <ul class="todo-list">
            <TodoItem 
                v-for="item in todoList" 
                :key="item.id" 
                :todo="item"
                @deleteTodo="deleteTodo"
                @changeStatusTodo="changeStatusTodo" 
                @editTodo="editTodo"
            />
        </ul>
    </div>
    <h2 class="todo-list__title" v-else>No tasks</h2>
</template>

<style lang="scss">
    .todo-list {
        width: 100%;
        &__container {
            display: flex;
            flex-direction: column;
            align-items: end;
        }
        &__title {
            text-align: center;
        }
        &__select {
            margin-bottom: 16px;
        }
    }
</style>