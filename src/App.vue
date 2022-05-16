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
    <!-- ======= Header ======= -->
    <header id="header" class="fixed-top">
      <div class="container d-flex align-items-center justify-content-between">
        <h1 class="logo"><a href="/">GameLog //</a></h1>
        <!-- Uncomment below if you prefer to use an image logo -->
        <!-- <a href="index.html" class="logo"><img src="assets/img/logo.png" alt="" class="img-fluid"></a>-->

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
        <!-- .navbar -->
      </div>
    </header>
    <!-- End Header -->
    <!-- <nav>
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
  </nav> -->
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
    <!-- End  Footer -->
  </div>
</template>
