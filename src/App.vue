<template>
  <div id="app">
    <div id="nav" v-if="isLoggedIn">
      <router-link to="/">Home</router-link> |
      <router-link to="/posts">Posts</router-link> |
      <router-link :to="`/posts?ownerId=${loggedUser.id}`">My Posts</router-link>
      <span>Olá, <router-link :to="`/users/${loggedUser.id}/edit`">{{ loggedUser.username }}</router-link></span>
      <button @click="logout">Logout</button>
    </div>
    <router-view :key="$route.fullPath"/>
  </div>
</template>

<script>
  export default {
    computed: {
      isLoggedIn() {
        return this.$store.getters.isLoggedIn
      },
      loggedUser() {
        return this.$store.getters.loggedUser
      }
    },
    methods: {
      async logout() {
        await this.$store.dispatch('logout')
        this.$router.push('/login')
      }
    },
    created() {
      this.$store.dispatch('fetchPosts')
      this.$store.dispatch('fetchUsers')
    }
  }
  </script>