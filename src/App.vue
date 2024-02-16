<script setup lang="ts">

import axios from 'axios';
import GitUserProfile from '@/components/GitUser/GitUserProfile.vue';
import GitUsernameInputVue from '@/components/GitUser/GitUsernameInput.vue';
import Notification from '@/components/Notification/Notification.vue';
import UserProfile from './components/UserProfile.vue';
import { ref } from 'vue';

const userData : Object = ref({});
const gitUsername  = ref('')
const notificationType  = ref(null)


// fetch userdata from the api
const fetchUserData = async (username : String ) => {
    try {
      const result = await axios(`https://api.github.com/users/${username}`);
      return {
      avatar: result.data.avatar_url,
      name: result.data.name,
      registerDate: result.data.created_at,
      bio: result.data.bio,
      followers: result.data.followers
      };
    } catch (err) {
      if (err.response && err.response.status === 404) {
      console.error(`User '${username}' not found`);
      return null;
    } else {
      console.error(err);
      throw err;
    }
  }
}

fetchUserData(gitUsername.value = 'rohitvispute2151').then((user) => {
  if (user) {
    userData.value = user
  }
});

// handle user form input data
async function handleSubmit(username){
  gitUsername.value = username;
  const user = await fetchUserData(username)
  if (user) {
    userData.value = user
    notificationType.value = "success"
  }else{
    notificationType.value = 'negative'
  }
}
</script>

<template>
  <div class="ui container">
      <!-- <UserProfile /> -->
      <Notification 
        v-if="notificationType !== null"
        :class="notificationType"
        :message="notificationType === 'success' ? 'Found The User' : 'User Not Found !'"
      />
      <GitUsernameInputVue 
        @username-submitted="handleSubmit"
      />
      <GitUserProfile 
        :avatar-url="userData?.avatar || '' "
        :user-name="userData?.name|| '' "
        :user-bio="userData?.bio|| '' "
        :user-followers="userData?.followers|| 0 "
        :user-registration-date="userData?.registerDate|| '' "
      />
  </div>
</template>

