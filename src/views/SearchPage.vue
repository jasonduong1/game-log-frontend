/* eslint-disable */
<script>
import axios from "axios";

export default {
  data: function () {
    return {
      input: "",
      results: [],
      currentGame: {},
    };
  },
  methods: {
    search: function () {
      console.log("search query for", this.input);
      axios.get("http://localhost:3000/games?search=" + this.input).then((response) => {
        this.results = response.data["results"];
        console.log(response.data);
        this.$router.push("/search#results");
      });
    },
  },
};
</script>

<template>
  <!-- <div id="hero" class="hero route bg-image" style="background-image: url(assets/img/hero-bg.jpg)">
    <div class="overlay-itro"></div>
    <div class="hero-content display-table">
      <div class="table-cell">
        <div class="container">
          <h1 class="hero-title mb-4">Search.</h1>
          <div class="small-search-wrap">
            <div class="search-form">
              <form v-on:submit.prevent="search()">
                <div class="form-group">
                  <input
                    type="text"
                    placeholder="Search games here ..."
                    maxlength="100"
                    class="form-control"
                    name="name"
                    id="name"
                    v-model="input"
                    href="#results"
                  />
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div> -->
  <section id="results">
    <div class="search mb-5 py-5">
      <hr />
      <div class="container">
        <div class="row">
          <div class="col-md">
            <div class="row text-left">
              <div class="col-md">
                <h3>Search.</h3>
                <div class="large-search-wrap">
                  <div class="search-form">
                    <form v-on:submit.prevent="search()">
                      <div class="form-group">
                        <input
                          type="text"
                          placeholder="Search games here ..."
                          maxlength="100"
                          class="form-control"
                          name="name"
                          id="name"
                          v-model="input"
                        />
                      </div>
                    </form>
                  </div>
                </div>
                <hr class="invisible small" />
              </div>
            </div>
            <div class="clearfix mt-40 col-md-10 offset-md-1 h-100 d-flex flex-column">
              <ul class="xsearch-items" v-for="result in results" :key="result">
                <div class="row flex-grow-1"></div>
                <li class="search-item">
                  <div class="search-item-img">
                    <a v-bind:href="`/games/${result.id}`">
                      <img :src="result.background_image" v-bind:alt="result.name" width="250" height="250" />
                    </a>
                  </div>
                  <div class="search-item-content">
                    <h3 class="search-item-caption">
                      <a v-bind:href="`/games/${result.id}`">{{ result.name }}</a>
                    </h3>

                    <div class="search-item-meta mb-15">
                      <ul class="list-inline">
                        <li class="time">Released: {{ result.released }}</li>
                      </ul>
                    </div>
                  </div>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- <form v-on:submit.prevent="search()">
    <input type="text" v-model="input" placeholder="Search for games..." />
    <input type="submit" value="Submit" />
  </form>
  <h1>Search results</h1>
  <div v-for="result in results" :key="result">
    <img :src="result.background_image" v-bind:alt="result.name" class="artwork" />
    <p>{{ result.name }}</p>
    <a v-bind:href="`/games/${result.id}`" class="btn btn-primary">More info</a>
    <br />
  </div> -->
  </section>
</template>
<style>
body {
  background: #f2f3f8;
  margin-top: 20px;
}

.page.has-sidebar.bordered .page-header {
  margin-bottom: 0;
}

.page.has-sidebar.bordered .page-inner {
  padding-bottom: 0;
  padding-top: 0px;
}

.page.has-sidebar.bordered .left-side-sidebar {
  padding-top: 50px;
  padding-bottom: 50px;
}

.page.has-sidebar.bordered .right-side-sidebar {
  padding-top: 50px;
  padding-bottom: 50px;
}

.has-sidebar.bordered.has-right-sidebar .left-side-sidebar {
  border-right: 1px solid rgba(0, 0, 0, 0.1) !important;
  box-shadow: 1px 0px 0px 0px rgba(255, 255, 255, 0.715) !important;
  padding-right: 45px;
}

.has-sidebar.bordered.has-right-sidebar .right-side-sidebar {
  padding-left: 40px;
}

.has-sidebar.bordered.has-left-sidebar .right-side-sidebar {
  padding-left: 45px;
}

.has-sidebar.bordered.has-left-sidebar .left-side-sidebar {
  border-right: 1px solid rgba(0, 0, 0, 0.1) !important;
  box-shadow: 1px 0px 0px 0px rgba(255, 255, 255, 0.715) !important;
  padding-right: 40px;
}

.page.has-sidebar .left-side-sidebar {
  padding-top: 50px;
  padding-bottom: 50px;
}

.page.has-sidebar .page-inner {
  padding-bottom: 0;
  padding-top: 0px;
}

.page.has-sidebar .right-side-sidebar {
  padding-top: 50px;
  padding-bottom: 50px;
}

/* --------------------------------------------
   SEARCH LIST
 -------------------------------------------- */

.xsearch-items {
  padding-left: 0px;
}

.search-item-img {
  float: left;
  position: relative;
}

.search-item-img img,
.search-item-img .img-holder {
  height: 15rem;
  width: 15rem;
  display: block;
  -moz-border-radius: 50%;
  -webkit-border-radius: 50%;
  border-radius: 50%;
  box-shadow: 0 1px 3px rgba(50, 50, 93, 0.15), 0 1px 0 rgba(0, 0, 0, 0.02);
  border: 4px solid white;
  object-fit: cover;
}

.search-item-img .img-holder {
  border: 1px solid #e3e3e3;
  line-height: 20px;
  background: #f7f7f7;
  border-color: rgba(207, 215, 223, 0.25);
}

.search-item-img .img-holder i {
  display: inline-block;
  padding: 28px 20px;
  font-size: 28px;
  opacity: 0.5;
}

.search-item-content {
  margin-left: 300px;
  padding-top: 100px;
  padding-bottom: 100px;
  /* margin-bottom: 120px; */
  /*border-bottom: 1px solid rgb(231, 231, 231);*/
}

.search-item-content.no-excerpt h3 {
  margin-top: 8px;
}

.search-item-content .search-item-meta {
  display: block;
  margin-bottom: 10px;
}

.search-item-meta-down a,
.search-item-meta a {
  font-size: 11px;
  text-transform: uppercase;
}

.xsearch-items a:hover {
  border-bottom-width: 1px;
  border-bottom-style: dotted;
}

.search-item-content .time {
  color: #999;
  font-size: 11px;
  text-transform: uppercase;
}

.search-item-content time,
.search-item-content span {
  color: #999999;
}

.xsearch-items h3.search-item-caption {
  margin-bottom: 2px;
  font-weight: 600;
  font-size: 20px;
}

.xsearch-items .search-item-meta-down {
  margin-top: 5px;
  color: #999;
  font-size: 12px;
}

.xsearch-items .star-vote li {
  padding: 0;
  font-size: 14px;
}

.xsearch-result-count {
  color: #999;
  margin-bottom: 30px;
}

ul.xsearch-items-2 {
  padding-left: 0;
  margin-left: 0;
}

.xsearch-items-2 li {
  list-style: none;
}

.xsearch-info-meta:before,
.xsearch-info-meta:after {
  content: "";
  display: table;
}

.xsearch-info-meta:after {
  clear: both;
}

.xsearch-info-meta {
  padding: 0;
  margin: 0;
  list-style-type: none;
  margin-bottom: 5px;
  font-size: 12px;
  opacity: 0.7;
}

.xsearch-info-meta-item {
  float: left;
  margin-right: 10px;
}

.xsearch-item-title h3,
.xsearch-item-title h4 {
  margin-bottom: 5px;
}

.xsearch-desc {
  margin-bottom: 2px;
}

.search-item-icon {
  padding-right: 3px;
}

nav.xsearch-navbar {
  padding-left: 0;
}

nav.xsearch-navbar.navbar-light .navbar-nav .active > .nav-link {
  border-bottom-width: 2px;
  border-bottom-style: solid;
  padding-bottom: 12px;
}

.xsearch-item .xsearch-item-title strong {
  font-weight: 600;
}

ul.xsearch-items-2 .xsearch-item {
  margin-bottom: 40px;
}

.xsearch-item .xsearch-desc strong {
  color: #111;
}

.search-result-wrap .search-result-item {
  padding-bottom: 25px;
  padding-top: 25px;
  border-bottom: 1px solid rgba(207, 215, 223, 0.25);
}

.search-result-wrap .search-result-item .title h4,
.search-result-wrap .search-result-item .title h3 {
  margin-bottom: 5px;
}

.search-result-item-meta {
  font-size: 14px;
  margin-bottom: 10px;
  margin-left: 25px;
}

.search-result-item-meta li {
  color: #999;
  margin-right: 5px;
}

.search-result-item-meta li i {
  margin-right: 4px;
}

.search-result-item-meta li a {
  /*border-bottom:1px dotted rgba(207,215,223,.25);
    padding-bottom:2px;*/
  color: #1a0dab;
}

.search-result-item-link,
.search-result-item-excerpt,
.search-result-item-meta {
  margin-left: 25px;
}

.search-result-item-excerpt strong {
  color: #444;
  font-weight: 600;
}

.search-result-item-excerpt {
  margin-bottom: 5px;
}

.search-result-item-meta li:first-child {
  margin-left: 0;
  padding-left: 0;
}

ul {
  list-style-type: none;
}
</style>
