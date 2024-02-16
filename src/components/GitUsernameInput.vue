<script setup>
import { ref } from 'vue';
const emit = defineEmits(['usernameSubmitted'])
const username = ref('')
function handleSubmit(){
    emit('usernameSubmitted',username.value)
    username.value = ''
}
</script>
<template>
    <form class="ui action input" @submit.prevent="handleSubmit">
        <input type="text" placeholder="Search..." v-model="username">
        <button class="ui icon button">
            <i class="search icon"></i>
        </button>
    </form>
</template>
<style scoped>
    .input{
        margin-bottom: 1rem;
    }
</style>