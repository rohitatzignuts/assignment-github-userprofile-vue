<script setup>
    import Notification from './Notification/Notification.vue';
    import GitUserProfile from './GitUser/GitUserProfile.vue';
    import GitUsernameInput from './GitUser/GitUsernameInput.vue';
</script>

<template>
    <Notification 
        message
    />
    <GitUsernameInput />
    <GitUserProfile />
</template>