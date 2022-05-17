/* eslint-disable */
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
    axios.get("/libraries").then((response) => {
      this.entries = response.data;
      console.log(this.entries);
    });
  },
  methods: {
    editEntry: function (entry) {
      axios.get("/libraries/" + entry.id).then((response) => {
        this.currentEntry = response.data;
        console.log(this.currentEntry);
        // document.querySelector("#game-details").showModal();
      });
    },
    destroyEntry: function (entry) {
      axios.delete("/libraries/" + entry.id).then((response) => {
        console.log("entry destroy", response);
        this.$router.go();
      });
    },
    updateEntry: function (entry) {
      console.log("Update entry.");
      axios
        .patch("/libraries/" + entry.id, this.currentEntry)
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
  <div class="library mt-0 p-5 mb-5 mx-5">
    <hr />
    <ol>
      <table class="table">
        <thead class="thead-dark">
          <tr>
            <th scope="col">#</th>
            <th scope="col">Title</th>
            <th scope="col">Rating</th>
            <th scope="col">Progress</th>
            <th scope="col">edit/remove</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="entry in entries" :key="entry">
            <th scope="row"><li></li></th>
            <td>
              <a v-bind:href="`/games/${entry.game_id}`">{{ entry.title }}</a>
            </td>
            <td>{{ entry.rating }}</td>
            <td>{{ entry.progress }}</td>
            <td>
              <button
                class="btn btn-success btn-custom waves-effect waves-light m-b-5"
                v-on:click="destroyEntry(entry)"
              >
                Remove
              </button>
              <button
                type="button"
                class="btn btn-success btn-custom waves-effect waves-light m-b-5"
                data-bs-toggle="modal"
                data-bs-target="#game-details"
                v-on:click="editEntry(entry)"
              >
                Edit
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </ol>
    <!-- Modal -->
    <div class="modal fade" id="game-details" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog">
        <form v-on:submit.prevent="updateEntry(currentEntry)">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="exampleModalLabel">Editing // {{ currentEntry.title }}</h5>
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
              <!-- Progress -->
              <label>Progress</label>
              <select class="browser-default custom-select mb-4" v-model="currentEntry.progress">
                <option value="" disabled>Your Progress</option>
                <option value="0">{{ currentEntry.progress }}</option>
                <option value="Playing">Playing</option>
                <option value="Dropped">Dropped</option>
                <option value="Plan to play">Plan to play</option>
                <option value="Backlog">Backlog</option>
                <option value="Pre-ordered">Pre-ordered</option>
                <option value="Completed">Completed</option>
                <option value="100%">100%</option>
                <option value="Played enough">Played enough</option>
              </select>
              <br />
              <!-- Rating -->
              <label>Rating</label>
              <select class="browser-default custom-select mb-4" v-model="currentEntry.rating">
                <option value="" disabled>Your Rating</option>
                <option value="0">{{ currentEntry.rating }}</option>
                <option value="10">10</option>
                <option value="9">9</option>
                <option value="8">8</option>
                <option value="7">7</option>
                <option value="6">6</option>
                <option value="5">5</option>
                <option value="4">4</option>
                <option value="3">3</option>
                <option value="2">2</option>
                <option value="1">1</option>
              </select>

              <!-- Review -->
              <div class="form-group">
                <textarea
                  v-model="currentEntry.review"
                  class="form-control rounded-0"
                  id="exampleFormControlTextarea2"
                  rows="3"
                  placeholder="Review"
                ></textarea>
              </div>
              <br />
              <!-- Note -->
              <div class="form-group">
                <textarea
                  v-model="currentEntry.note"
                  class="form-control rounded-0"
                  id="exampleFormControlTextarea2"
                  rows="3"
                  placeholder="Note"
                ></textarea>
              </div>
            </div>
            <div class="modal-footer">
              <button
                type="button"
                class="btn btn-success btn-custom waves-effect waves-light m-b-5"
                data-bs-dismiss="modal"
              >
                Close
              </button>
              <button type="submit" class="btn btn-success btn-custom waves-effect waves-light m-b-5">
                Save changes
              </button>
            </div>
          </div>
        </form>
      </div>
    </div>
    <dialog id="game-details">
      <!-- Default form contact -->
      <form method="dialog" v-on:submit.prevent="updateEntry(currentEntry)">
        <p class="h4 mb-4">Edit Log</p>
        <h5>{{ currentEntry.title }}</h5>

        <!-- Progress -->
        <label>Progress</label>
        <select class="browser-default custom-select mb-4" v-model="currentEntry.progress">
          <option value="" disabled>Your Progress</option>
          <option value="0">{{ currentEntry.progress }}</option>
          <option value="Playing">Playing</option>
          <option value="Dropped">Dropped</option>
          <option value="Plan to play">Plan to play</option>
          <option value="Backlog">Backlog</option>
          <option value="Pre-ordered">Pre-ordered</option>
          <option value="Completed">Completed</option>
          <option value="100%">100%</option>
          <option value="Played enough">Played enough</option>
        </select>
        <br />
        <!-- Rating -->
        <label>Rating</label>
        <select class="browser-default custom-select mb-4" v-model="currentEntry.rating">
          <option value="" disabled>Your Rating</option>
          <option value="0">{{ currentEntry.rating }}</option>
          <option value="10">10</option>
          <option value="9">9</option>
          <option value="8">8</option>
          <option value="7">7</option>
          <option value="6">6</option>
          <option value="5">5</option>
          <option value="4">4</option>
          <option value="3">3</option>
          <option value="2">2</option>
          <option value="1">1</option>
        </select>

        <!-- Review -->
        <div class="form-group">
          <textarea
            v-model="currentEntry.review"
            class="form-control rounded-0"
            id="exampleFormControlTextarea2"
            rows="3"
            placeholder="Review"
          ></textarea>
        </div>
        <br />
        <!-- Note -->
        <div class="form-group">
          <textarea
            v-model="currentEntry.note"
            class="form-control rounded-0"
            id="exampleFormControlTextarea2"
            rows="3"
            placeholder="Note"
          ></textarea>
        </div>
        <br />
        <!-- Send button -->
        <table class="table">
          <tbody>
            <tr>
              <td scope="col"><a href="/libraries">Back to log</a></td>
              <td scope="col"><button class="btn btn-info btn-block" type="submit">Update</button></td>
            </tr>
          </tbody>
        </table>
      </form>
    </dialog>
  </div>
</template>
<style>
th {
  color: aliceblue;
}
td {
  color: aliceblue;
}
.modal-header {
  background-color: rgb(0, 0, 0, 0.7);
}
.modal-footer {
  background-color: rgb(0, 0, 0, 0.7);
}
.modal-content {
  background-color: rgba(0, 0, 0, 0.9);
}
</style>
