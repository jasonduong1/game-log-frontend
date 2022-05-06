<script>
import axios from "axios";

export default {
  data: function () {
    return {
      input: "",
      results: [],
    };
  },
  methods: {
    search: function () {
      console.log(this.input);
      axios.get("http://localhost:3000/games?search=" + this.input).then((response) => {
        this.results = response.data["results"];
        console.log(this.results);
      });
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
  </div>
</template>
