<script>
export default {
  data: function () {
    return {
      isLoggedIn: !!localStorage.jwt,
    };
  },
  watch: {
    $route: function () {
      this.isLoggedIn = !!localStorage.jwt;
    },
  },
};
</script>
<template>
  <nav>
    <router-link to="/">Home</router-link>
    |
    <router-link to="/search">Search</router-link>
    <span v-if="!isLoggedIn">
      |
      <router-link to="/signup">Signup</router-link>
      |
      <router-link to="/login">Login</router-link>
    </span>
    <span v-if="isLoggedIn">
      |
      <router-link to="/libraries">Game Log</router-link>
      |
      <router-link to="/logout">Logout</router-link>
    </span>
  </nav>
  <router-view />
</template>

<style></style>
