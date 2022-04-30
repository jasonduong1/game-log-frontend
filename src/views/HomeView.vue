<script>
import axios from "axios";

export default {
  data: function () {
    return {
      games: [],
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
};
</script>

<template>
  <h1>Games</h1>
  <div v-for="game in games" :key="game.id">
    <img :src="game.background_image" v-bind:alt="game.name" class="artwork" />
    <h3>{{ game.name }}</h3>
    <p>Released: {{ game.released }}</p>
  </div>
</template>

<style>
img {
  height: 300px;
  object-fit: cover;
}
</style>
