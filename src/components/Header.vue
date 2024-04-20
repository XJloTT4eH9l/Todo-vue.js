<script setup lang="ts">
    defineProps({
        title: String
    })
</script>

<template>
    <header class="header">
        <h1 class="header__title">{{ title }}</h1>
    </header>
</template>

<style scoped>
    .header {
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .header__title {
        font-size: 30px;
        font-weight: 500;
    }
</style>