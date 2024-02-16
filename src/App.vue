<script setup>

import axios from 'axios';
import GitUserProfile from '@/components/GitUser/GitUserProfile.vue';
import GitUsernameInputVue from '@/components/GitUser/GitUsernameInput.vue';
import Notification from '@/components/Notification/Notification.vue';
import { ref } from 'vue';

const userData = ref([]);
const gitUsername = ref('rohitvispute2151')
const notificationType = ref(null)
// fetch userdata from the api
function fetchUserData(username) {
  return axios(`https://api.github.com/users/${username}`)
    .then((result) => {
      return {
        avatar: result.data.avatar_url,
        name: result.data.name,
        registerDate: result.data.created_at,
        bio: result.data.bio,
        followers: result.data.followers
      };
    })
    .catch((err) => {
      if (err.response && err.response.status === 404 ) {
        console.error(`User '${username}' not found`);
        return null
      } else {
        console.error(err)
        throw err
      }
    });
}

fetchUserData(gitUsername.value).then((user) => {
  if (user) {
    userData.value = [user]
  }
});

// handle user form input data
async function handleSubmit(username){
  gitUsername.value = username;
  const user = await fetchUserData(username)
  if (user) {
    userData.value = [user]
    notificationType.value = "success"
  }else{
    notificationType.value = 'negative'
  }
}
</script>

<template>
  <div class="ui container">
      <Notification 
        v-if="notificationType !== null"
        :class="notificationType"
        :message="notificationType === 'success' ? 'Found The User' : 'User Not Found !'"
      />
      <GitUsernameInputVue 
        @username-submitted="handleSubmit"
      />
      <GitUserProfile 
        :avatar-url="userData[0]?.avatar"
        :user-name="userData[0]?.name"
        :user-bio="userData[0]?.bio"
        :user-followers="userData[0]?.followers"
        :user-registration-date="userData[0]?.registerDate"
      />
  </div>
</template>

