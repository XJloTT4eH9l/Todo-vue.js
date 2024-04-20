<script setup>
    import { ref, onMounted, watch } from 'vue';

    import Header from './components/Header.vue'
    import Form from './components/Form.vue'
    import TodoList from './components/TodoList.vue'

    const todoList = ref([]);

    watch(todoList, (newList) => {
        localStorage.setItem('todoList', JSON.stringify(newList));
    }, {deep: true});

    onMounted(() => {
        const savedTodoList = localStorage.getItem("todoList");
        if(savedTodoList) {
            todoList.value = JSON.parse(savedTodoList);
        }
    });
</script>

<template>
    <main class="wrapper">
    <div class="app">
        <Header title="Todo list" />
        <Form v-model:todoList="todoList" />
        <TodoList v-model:todoList="todoList" />
    </div>
    </main>
</template>

<style scoped>
    .wrapper {
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 0 15px;
    }
    .app {
        width: 100%;
        max-width: 800px;
    }
</style>
