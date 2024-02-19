<script setup lang="ts">
import { defineProps, onMounted } from 'vue';

defineProps({
    message : {
        type : String,
        required : true
    }
});

onMounted(() => {
    $('.message .close').on('click', function() {
        $(this).closest('.message').transition('fade')
    });
});
</script>

<template>
    <!-- Notification Message Markup -->
    <!-- Types: info, warning, error, success -->
    <div class="ui message">
        <i class="close icon"></i>
        <div class="header">
            {{ message }}
        </div>
    </div>
</template>
