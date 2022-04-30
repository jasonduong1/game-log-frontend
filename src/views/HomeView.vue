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
    showGame: function (game) {
      console.log(game.id);
      axios.get("http://localhost:3000/games/" + game.id).then((response) => {
        this.currentGame = response.data;
        console.log(this.currentGame);
        document.querySelector("#game-details").showModal();
      });
    },
  },
};
</script>

<template>
  <h1>Games</h1>
  <div v-for="game in games" :key="game.id">
    <img :src="game.background_image" v-bind:alt="game.name" class="artwork" />
    <h3>{{ game.name }}</h3>
    <button v-on:click="showGame(game)">info</button>
  </div>
  <dialog id="game-details">
    <form method="dialog">
      <img id="dialog" v-bind:src="currentGame.background_image" :alt="currentGame.name" />
      <h3>{{ currentGame.name }}</h3>
      <h4>{{ currentGame.released }}</h4>
      <p>{{ currentGame.description }}</p>
      <button>Close</button>
    </form>
  </dialog>
</template>

<style>
img {
  height: 300px;
  object-fit: cover;
}
</style>
