<script>
import axios from "axios";

export default {
  data: function () {
    return {
      game: {},
      currentGame: {},
    };
  },
  created: function () {
    axios.get("http://localhost:3000/games/" + this.$route.params.id).then((response) => {
      console.log(response.data);
      this.game = response.data;
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
  <h1>{{ game.name }}</h1>
  <img :src="game.background_image" v-bind:alt="game.name" class="artwork" />
  <h4>Released: {{ game.released }}</h4>
  <p>{{ game.description_raw }}</p>
  <!-- when logged in -->
  <!-- button to add to library -->
  <button v-on:click="addLibrary(game)">Add to library</button>
  <!-- when logged in and title added -->
  <!-- remove button replaces add button -->
  <!-- body for progress -->
  <!-- body for rating -->
  <!-- body for review -->
  <!-- body for note -->
  <!-- edit button -->
</template>
