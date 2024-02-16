<script setup lang="ts">
import { ref } from 'vue';
const emit = defineEmits(['usernameSubmitted'])
const username : String = ref('')
function handleSubmit(){
    emit('usernameSubmitted',username.value)
    username.value = ''
}
</script>
<template>
    <form class="ui action input" @submit.prevent="handleSubmit">
        <input type="text" placeholder="Github UserName..." v-model="username" required>
        <button class="ui icon button">
            <i class="search icon"></i>
        </button>
    </form>
</template>
