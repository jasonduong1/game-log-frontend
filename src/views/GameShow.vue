<script>
import axios from "axios";

export default {
  data: function () {
    return {
      game: {},
    };
  },
  created: function () {
    axios.get("http://localhost:3000/games/" + this.$route.params.id).then((response) => {
      console.log(response.data);
      this.game = response.data;
    });
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
  <!-- when logged in and title added -->
  <!-- remove button replaces add button -->
  <!-- body for progress -->
  <!-- body for rating -->
  <!-- body for review -->
  <!-- body for note -->
  <!-- edit button -->
</template>
