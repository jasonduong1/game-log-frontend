/* eslint-disable */
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
  <div
    id="app"
    :style="{
      'background-image': 'url(' + require('./assets/nami.webp') + ')',
    }"
  >
    <header id="header" class="fixed-top">
      <div class="container d-flex align-items-center justify-content-between">
        <h1 class="logo"><a href="/">GameLog //</a></h1>

        <nav id="navbar" class="navbar">
          <ul>
            <li><a class="nav-link scrollto" href="/search">Search</a></li>
            <li v-if="!isLoggedIn"><a class="nav-link scrollto" href="/signup">Signup</a></li>
            <li v-if="!isLoggedIn"><a class="nav-link scrollto" href="/login">Login</a></li>
            <li v-if="isLoggedIn"><a class="nav-link scrollto" href="/libraries">My Library</a></li>
            <li v-if="isLoggedIn"><a class="nav-link scrollto" href="/logout">Logout</a></li>
          </ul>
          <i class="bi bi-list mobile-nav-toggle"></i>
        </nav>
      </div>
    </header>
    <router-view />
    <footer>
      <div class="container">
        <div class="row">
          <div class="col-sm-12">
            <div class="copyright-box">
              <p class="copyright">
                &copy; Copyright
                <strong>GameLog</strong>
                . All Rights Reserved
              </p>
            </div>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>
