/* eslint-disable */
<script>
import axios from "axios";

export default {
  data: function () {
    return {
      games: [],
      currentGame: {},
      entries: [],
      nextPage: "",
      previousPage: "",
      isLoggedIn: !!localStorage.jwt,
      isInLibrary: false,
      gameIDs: [],
    };
  },
  created: function () {
    axios.get("/games").then((response) => {
      this.games = response.data["results"];
      this.nextPage = response.data["next"];
      this.previousPage = response.data["previous"];
    });
    if (this.isLoggedIn) {
      axios.get("/libraries").then((response) => {
        this.entries = response.data;
        this.entries.forEach((i) => {
          this.gameIDs.push(i.game_id);
        });
      });
      console.log("gameIDs", this.gameIDs);
    }
  },
  watch: {
    $route: function () {
      this.isLoggedIn = !!localStorage.jwt;
    },
  },
  methods: {
    addLibrary: function (game) {
      (this.currentGame.game_id = game.id),
        (this.currentGame.title = game.name),
        (this.currentGame.image = game.background_image),
      axios
        .post("/libraries", this.currentGame)
        .then((response) => {
          console.log("Success", response.data);
          this.$router.go();
        })
        .catch((error) => console.log(error.response));
    },
    inLibrary: function (game) {
      return this.gameIDs.includes(game.id);
    },
    redirectLogin: function () {
      this.$router.push("/login");
    },
  },
};
</script>

<template>
  <div class="app">
    <section id="popular" class="portfolio-mf sect-pt4 route">
      <span v-if="isLoggedIn"><hr /></span>
      <div class="container mt-0 pt-3">
        <div class="row">
          <div class="col-sm-12">
            <div class="title-box text-center">
              <h3 class="title-a">Popular Games</h3>
              <p v-if="!isLoggedIn" class="subtitle-a">Click on title to start building your game log.</p>
              <div class="line-mf"></div>
            </div>
          </div>
        </div>
        <div class="row">
          <div v-for="game in games" :key="game.id" class="col-md-4">
            <div class="work-box">
              <a v-bind:href="`/games/${game.id}`" data-gallery="portfolioGallery" class="portfolio-lightbox">
                <div class="work-img">
                  <img :src="game.background_image" v-bind:alt="game.name" class="img-fluid" />
                </div>
              </a>
              <div class="work-content">
                <div class="row">
                  <div class="col-sm-8">
                    <h2 class="w-title">
                      <a v-bind:href="`/games/${game.id}`">{{ game.name }}</a>
                    </h2>
                    <div class="w-more">
                      <span class="w-ctegory">Released</span>
                      /
                      <span class="w-date">{{ game.released }}</span>
                    </div>
                  </div>
                  <div class="col-sm-4">
                    <div class="w-like" v-if="!isLoggedIn">
                      <span v-on:click="redirectLogin" class="bi bi-plus-circle"></span>
                    </div>
                    <div class="w-like" v-if="isLoggedIn">
                      <span v-if="!inLibrary(game)">
                        <span v-on:click="addLibrary(game)" class="bi bi-plus-circle"></span>
                      </span>
                    </div>
                    <div class="w-like2" v-if="isLoggedIn">
                      <span v-if="inLibrary(game)">
                        <i class="bi bi-check-circle"></i>
                      </span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<style>
.btn {
  -moz-box-shadow: 0px 1px 2px 0px rgba(0, 0, 0, 0.1);
  -webkit-box-shadow: 0px 1px 2px 0px rgba(0, 0, 0, 0.1);
  border-radius: 2px;
  box-shadow: 0px 1px 2px 0px rgba(0, 0, 0, 0.1);
  font-family: "Nunito", sans-serif;
  letter-spacing: 0.2px;
  opacity: 0.93;
}
.btn:hover {
  -moz-box-shadow: 0 2px 5px 0 rgba(0, 0, 0, 0.16), 0 2px 10px 0 rgba(0, 0, 0, 0.12);
  -webkit-box-shadow: 0 2px 5px 0 rgba(0, 0, 0, 0.16), 0 2px 10px 0 rgba(0, 0, 0, 0.12);
  box-shadow: 0 2px 5px 0 rgba(0, 0, 0, 0.16), 0 2px 10px 0 rgba(0, 0, 0, 0.12);
  opacity: 1;
}
.btn:focus {
  -moz-box-shadow: 0 2px 5px 0 rgba(0, 0, 0, 0.16), 0 2px 10px 0 rgba(0, 0, 0, 0.12);
  -webkit-box-shadow: 0 2px 5px 0 rgba(0, 0, 0, 0.16), 0 2px 10px 0 rgba(0, 0, 0, 0.12);
  box-shadow: 0 2px 5px 0 rgba(0, 0, 0, 0.16), 0 2px 10px 0 rgba(0, 0, 0, 0.12);
  opacity: 1;
}
.btn-success {
  color: #ffffff !important;
}
.btn-custom.btn-inverse {
  color: #212121 !important;
}
.btn-custom.btn-success {
  color: #0dff00 !important;
}
.btn-custom {
  -moz-border-radius: 2px;
  -moz-transition: all 400ms ease-in-out;
  -o-transition: all 400ms ease-in-out;
  -webkit-border-radius: 2px;
  -webkit-transition: all 400ms ease-in-out;
  background: transparent;
  background-color: transparent !important;
  border-radius: 2px;
  border-width: 1px;
  transition: all 400ms ease-in-out;
}
.btn-custom:hover {
  color: #ffffff !important;
}
.btn-custom:focus {
  color: #ffffff !important;
}
</style>
