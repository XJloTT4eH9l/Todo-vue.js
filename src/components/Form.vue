<script setup>
    import { ref } from 'vue';

    const props = defineProps({
        submitHandler: Function,
        sortOptions: Array
    })

    const text = ref("");
    const priority = ref("low");

    const handleSubmit = () => {
        if(text.value.length === 0) {
            return
        }

        props.submitHandler({
            id: Date.now(),
            task: text.value, 
            priority: priority.value,
            completed: false
        });
        
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
    <div class="options">
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

<style>
    .form {
        display: flex;
        flex-direction: column;
        gap: 10px;
        padding: 30px;
        border: 2px solid black;
        border-radius: 6px;
        margin-bottom: 16px;
    }
    .form__top {
        display: flex;
        align-items: center;
        gap: 10px;
    }
    .options {
        display: flex;
        flex-direction: column;
    }
    .options__item {
        display: flex;
        gap: 4px;
        align-items: center;
    }
    .input {
        width: 50%;
        outline: none;
        padding: 8px 12px;
        border: 1px solid black;
        border-radius: 6px;
        background-color: transparent;
    }
    .btn {
        cursor: pointer;
        background-color: transparent;
        padding: 8px 12px;
        background-color: black;
        color: white;
        outline: none;
        border-radius: 6px;
        border: none;
        transition: scale 0.2s ease-in-out;
    }
    .btn:hover {
        scale: 1.05;
    }
</style>