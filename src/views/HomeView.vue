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
    axios.get("http://localhost:3000/games").then((response) => {
      this.games = response.data["results"];
      this.nextPage = response.data["next"];
      this.previousPage = response.data["previous"];
      console.log("games", this.games);
      console.log("next page:", this.nextPage);
      console.log("previous page:", this.previousPage);
    });
    if (this.isLoggedIn) {
      axios.get("http://localhost:3000/libraries").then((response) => {
        console.log("entries", response.data);
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
        console.log("new entry", this.currentGame);
      axios
        .post("http://localhost:3000/libraries", this.currentGame)
        .then((response) => {
          console.log("Success", response.data);
        })
        .catch((error) => console.log(error.response));
    },
    inLibrary: function (game) {
      return this.gameIDs.includes(game.id);
    },
  },
};
</script>

<template>
  <h1>Games</h1>
  <div v-for="game in games" :key="game.id">
    <img :src="game.background_image" v-bind:alt="game.name" class="artwork" />
    <h3>{{ game.name }}</h3>
    <a v-bind:href="`/games/${game.id}`" class="btn btn-primary">More info</a>
    <br />
    <!-- button to add to library when logged in -->
    <span v-if="isLoggedIn">
      <span v-if="!inLibrary(game)">
        <button v-on:click="addLibrary(game)">Add to library</button>
      </span>
      <span v-if="inLibrary(game)">
        <button>Game added</button>
      </span>
    </span>
  </div>
</template>

<style>
img {
  height: 300px;
  object-fit: cover;
}
</style>
