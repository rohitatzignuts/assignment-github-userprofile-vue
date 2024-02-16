<script setup>
defineProps({
    avatarUrl : {
        type : String,
        required : true
    },
    userName : {
        type : String,
        required : true
    },
    userRegistrationDate : {
        type : String,
        required : true
    },
    avatarUrl : {
        type : String,
        required : true
    },
    userBio : {
        type : String,
        required : true
    },
    userFollowers : {
        type : Number,
        required : true
    },
});
</script>

<template>
    <div class="ui card">
    <div class="image">
        <img :src="avatarUrl">
    </div>
    <div class="content">
        <a class="header">{{userName}}</a>
        <div class="meta">
        <span class="date">Joined in {{userRegistrationDate}}</span>
        </div>
        <div class="description">
        {{userBio}}
        </div>
    </div>
    <div class="extra content">
        <a>
        <i class="user icon"></i>
        {{userFollowers}} Followers
        </a>
    </div>
</div>

</template>