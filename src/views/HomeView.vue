<script>
import axios from "axios";

export default {
  data: function () {
    return {
      games: [],
      currentGame: {},
      nextPage: "",
      previousPage: "",
    };
  },
  created: function () {
    axios.get("http://localhost:3000/games").then((response) => {
      this.games = response.data["results"];
      this.nextPage = response.data["next"];
      this.previousPage = response.data["previous"];
      console.log(this.games);
      console.log(this.nextPage);
      console.log(this.previousPage);
    });
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
    <button v-on:click="addLibrary(game)">Add to library</button>
  </div>
</template>

<style>
img {
  height: 300px;
  object-fit: cover;
}
</style>
