<script>
import axios from "axios";

export default {
  data: function () {
    return {
      entries: [],
      currentEntry: {},
    };
  },
  created: function () {
    axios.get("http://localhost:3000/libraries").then((response) => {
      this.entries = response.data;
      console.log(this.entries);
    });
  },
  methods: {
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
          this.$router.push("/libraries");
        })
        .catch((error) => console.log(error.response));
    },
  },
};
</script>

<template>
  <h1>Game Log</h1>
  <ol>
    <div v-for="entry in entries" :key="entry">
      <li>
        title:
        {{ entry.title }}
        <a v-bind:href="`/games/${entry.game_id}`" class="btn btn-primary">Info</a>
        rating:
        {{ entry.rating }}
        progress:
        {{ entry.progress }}
        <button v-on:click="destroyEntry(entry)">Remove</button>
        <button v-on:click="editEntry(entry)">edit</button>
      </li>
      <dialog id="game-details">
        <form method="dialog" v-on:submit.prevent="updateEntry(currentEntry)">
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
    </div>
  </ol>
</template>
