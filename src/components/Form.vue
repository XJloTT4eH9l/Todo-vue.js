<script setup>
    import { ref } from 'vue';

    const text = ref("");
    const priority = ref("low");
    const sortOptions = ['low', 'high'];
    const todoList = defineModel('todoList');

    const handleSubmit = () => {
        if(text.value.length === 0) {
            return
        }

        const newTodoItem = {
            id: Date.now(),
            task: text.value, 
            priority: priority.value,
            completed: false
        }

        todoList.value.push(newTodoItem);
        text.value = '';
    }
</script>

<template>
    <form class="form" @submit.prevent="handleSubmit">
        <div class="form__top">
            <input class="input" v-model="text" type="text">
            <button class="btn" type="submit">Add todo</button>
        </div>
        <div>Priority:</div>
        <div class="form__options">
            <div v-for="option in sortOptions" :key="option" class="options__item">
                <input 
                    type="radio" 
                    :id="`${option}-option`" 
                    :value="option"
                    v-model="priority"
                >
                <label :for="`${option}-option`">{{ option }}</label>
            </div>
        </div>
    </form>
</template>

<style lang="scss">
    .form {
        display: flex;
        flex-direction: column;
        gap: 10px;
        padding: 30px;
        border: 2px solid black;
        border-radius: 6px;
        margin-bottom: 16px;
        &__top {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        &__options {
            display: flex;
            flex-direction: column;
            &__item {
                display: flex;
                gap: 4px;
                align-items: center;
            }
        }
    }
</style>