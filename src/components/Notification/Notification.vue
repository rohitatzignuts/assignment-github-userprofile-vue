<script setup>

import { onMounted } from 'vue'
onMounted( () => {
    $('.message .close').on('click', function() {
            $(this).closest('.message').transition('fade')
    });
})
defineProps(['message']);

</script>

<template>
    <!-- Notification Message Markup -->
    <!-- Types: info, warning, error, success -->
    <div class="ui message">
        <i class="close icon"></i>
        <div class="header">
            {{message}}
        </div>
    </div>
</template>