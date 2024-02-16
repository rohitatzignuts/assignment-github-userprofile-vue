<script setup>

import axios from 'axios';
import GitUserProfile from '@/components/GitUserProfile.vue';
import GitUsernameInputVue from './components/GitUsernameInput.vue';
import { ref } from 'vue';

const userData = ref([]);
const gitUsername = ref('rohitvispute2151')

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
  }else{
    return "User Not Found"
  }
}
</script>

<template>
  <div class="ui container">
    <GitUsernameInputVue 
      @username-submitted="handleSubmit"
    />
    <div v-if="userData.length">
      <GitUserProfile 
        :avatar-url="userData[0]?.avatar"
        :user-name="userData[0]?.name"
        :user-bio="userData[0]?.bio"
        :user-followers="userData[0]?.followers"
        :user-registration-date="userData[0]?.registerDate"
      />
    </div>
    <code v-else>404 | Not Found</code>
  </div>

</template>

