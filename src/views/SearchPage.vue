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
        console.log(this.results);
      });
    },
    addLibrary: function (result) {
      (this.currentGame.game_id = result.id),
        (this.currentGame.title = result.name),
        (this.currentGame.image = result.background_image),
        console.log("title to be added", this.currentGame);
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
  <form v-on:submit.prevent="search()">
    <input type="text" v-model="input" placeholder="Search for games..." />
    <input type="submit" value="Submit" />
  </form>
  <h1>Search results</h1>
  <div v-for="result in results" :key="result">
    <img :src="result.background_image" v-bind:alt="result.name" class="artwork" />
    <p>{{ result.name }}</p>
    <a v-bind:href="`/games/${result.id}`" class="btn btn-primary">More info</a>
    <br />
    <!-- button to add to library when logged in -->
    <button v-on:click="addLibrary(result)">Add to library</button>
  </div>
</template>
