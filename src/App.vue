<script setup>
    import { ref, onMounted, watch } from 'vue';

    import Header from './components/Header.vue'
    import Form from './components/Form.vue'
    import TodoList from './components/TodoList.vue'

    const todoList = ref([]);
    const sortOptions = ['low', 'high'];

    const addNewTodo = (todoItem) => {
        todoList.value.push(todoItem);
    }

    const deleteTodo = (id) => {
        todoList.value =  todoList.value.filter(todo => todo.id !== id);
    }

    const changeStatusTodo = (id) => {
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

    const editTodo = (id, value) => {
        todoList.value = todoList.value.map(todo => {
            if(todo.id === id) {
                return {...todo, task: value}
            } else {
                return todo
            }
        });
    }

    const sortTodos = (option) => {
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
        <Form 
            :submitHandler="addNewTodo"
            :sortOptions 
        />
        <TodoList 
            :todoList
            :deleteTodo
            :changeStatusTodo
            :sortTodos
            :editTodo
        />
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
