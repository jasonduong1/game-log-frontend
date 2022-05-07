<script>
import axios from "axios";

export default {
  data: function () {
    return {
      game: {},
      currentGame: {},
      entry: {},
      currentEntry: {},
      isLoggedIn: !!localStorage.jwt,
    };
  },
  watch: {
    $route: function () {
      this.isLoggedIn = !!localStorage.jwt;
    },
  },
  created: function () {
    axios.get("http://localhost:3000/games/" + this.$route.params.id).then((response) => {
      console.log(response.data);
      this.game = response.data;
    });
    axios.get("http://localhost:3000/libraries?game_id=" + this.$route.params.id).then((response) => {
      console.log(response.data);
      this.entry = response.data;
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
    editEntry: function (entry) {
      axios.get("http://localhost:3000/libraries/" + entry.id).then((response) => {
        this.currentEntry = response.data;
        console.log(this.currentEntry);
        document.querySelector("#game-details").showModal();
      });
    },
    destroyEntry: function (entry) {
      axios.delete("http://localhost:3000/libraries/" + entry.id).then((response) => {
        console.log("entry destroy", response);
        this.$router.go();
      });
    },
    updateEntry: function (entry) {
      console.log("Update entry.");
      axios
        .patch("http://localhost:3000/libraries/" + entry.id, this.currentEntry)
        .then((response) => {
          console.log("Edited", response.data);
          this.$router.go();
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
  <span v-if="isLoggedIn">
    <button v-on:click="addLibrary(game)">Add to library</button>
    <!-- when logged in and title added -->
    <!-- remove button replaces add button -->
    <!-- body for user progress, etc -->
    <p>Progress: {{ entry.progress }}</p>
    <p>Rating: {{ entry.rating }}</p>
    <p>Review: {{ entry.review }}</p>
    <p>Thoughts/notes: {{ entry.note }}</p>
    <!-- edit button -->
    <button v-on:click="destroyEntry(entry)">Remove</button>
    <button v-on:click="editEntry(entry)">edit</button>
  </span>
  <dialog id="game-details">
    <form method="dialog" v-on:submit.prevent="updateEntry(entry)">
      <h2>Edit Log</h2>
      <h3>{{ currentEntry.title }}</h3>
      <div>
        Progress:
        <input type="text" v-model="currentEntry.progress" />
      </div>
      <div>
        Rating:
        <input type="text" v-model="currentEntry.rating" />
      </div>
      <div>
        Review:
        <input type="text" v-model="currentEntry.review" />
      </div>
      <div>
        Thoughts/notes:
        <input type="text" v-model="currentEntry.note" />
        <br />
        <input type="submit" value="Update" />
        <!-- option: add update status text when update button is pressed -->
      </div>
      <a href="/libraries">Back to log</a>
    </form>
  </dialog>
</template>
