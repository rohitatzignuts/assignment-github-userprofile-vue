<script setup lang="ts">

import axios from 'axios';
import GitUserProfile from './GitUser/GitUserProfile.vue';
import GitUsernameInputVue from './GitUser/GitUsernameInput.vue';
import Notification from './Notification/Notification.vue';
import { ref } from 'vue';

interface UserData {
  avatar: string;
  name: string;
  registerDate: string;
  bio: string;
  followers: number;
}

const userData = ref<UserData>({
  avatar: '',
  name: '',
  registerDate: '',
  bio: '',
  followers: 0
});

const gitUsername = ref<string>('');
const notificationType = ref<string | null>(null);

// fetch userdata from the api
const fetchUserData = async (username: string) => {
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
};

fetchUserData(gitUsername.value = 'rohitvispute2151').then((user) => {
  if (user) {
    userData.value = user;
  }
});

// handle user form input data
async function handleSubmit(username: string) {
  gitUsername.value = username;
  const user = await fetchUserData(username);
  if (user) {
    userData.value = user;
    notificationType.value = "success";
  } else {
    notificationType.value = 'negative';
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
        :avatar-url="userData?.avatar || '' "
        :user-name="userData?.name|| '' "
        :user-bio="userData?.bio|| '' "
        :user-followers="userData?.followers|| 0 "
        :user-registration-date="userData?.registerDate|| '' "
      />
  </div>
</template>

