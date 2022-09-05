/* eslint-disable */
<script>
import axios from "axios";

export default {
  data: function () {
    return {
      game: {},
      currentGame: {},
      entry: {},
      currentEntry: {},
      genres: [],
      isLoggedIn: !!localStorage.jwt,
      isInLibrary: false,
      onEdit: false,
    };
  },
  created: function () {
    axios.get("/games/" + this.$route.params.id).then((response) => {
      this.game = response.data;
      this.genres = response.data["genres"];
      console.log("genres", this.genres);
    });
    if (this.isLoggedIn) {
      axios.get("/libraries?game_id=" + this.$route.params.id).then((response) => {
        this.entry = response.data;
        this.isInLibrary = !!this.entry;
      });
    }
  },
  watch: {
    $route: function () {
      this.isLoggedIn = !!localStorage.jwt;
    },
  },
  methods: {
    addLibrary: function (game) {
      (this.currentGame.game_id = game.id),
        (this.currentGame.title = game.name),
        (this.currentGame.image = game.background_image),
      axios
        .post("/libraries", this.currentGame)
        .then((response) => {
          console.log("Success", response.data);
          this.$router.go();
        })
        .catch((error) => console.log(error.response));
    },
    editEntry: function () {
      this.onEdit = true;
    },
    editStop: function () {
      this.$router.go();
    },
    redirectLogin: function () {
      this.$router.push("/login");
    },
    destroyEntry: function (entry) {
      axios.delete("/libraries/" + entry.id).then((response) => {
        console.log("entry destroy", response);
        this.$router.go();
      });
    },
    updateEntry: function (entry) {
      axios
        .patch("/libraries/" + entry.id, this.entry)
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
  <div class="show mb-5 py-5">
    <hr />
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.12.1/css/all.min.css"
      integrity="sha256-mmgLkCYLUQbXn0B1SRqzHar6dCnv9oZFPEC1g1cwlkk="
      crossorigin="anonymous"
    />
    <div class="container">
      <div class="row">
        <div class="col-lg-5 col-md-6">
          <div class="mb-2">
            <img class="w-100" :src="game.background_image" v-bind:alt="game.name" />
            <p v-if="isInLibrary">
              <i class="bi bi-check-circle"></i>
              In library
            </p>
          </div>
          <div class="mb-1 d-flex">
            <h4 class="font-weight-normal">{{ game.name }}</h4>
          </div>
          <div class="mb-2">
            <ul class="list-unstyled">
              <li class="media">
                <span class="w-25 text-grey font-weight-normal">Released /</span>
                <label class="media-body">{{ game.released }}</label>
              </li>
              <li class="media">
                <span class="w-25 text-grey font-weight-normal">Genre /</span>
                <label class="media-body" v-for="genre in genres" :key="genre.name">
                  {{ genre.name }}
                </label>
              </li>
              <li class="media">
                <span v-if="!isLoggedIn">
                  <button
                    class="btn btn-success btn-custom waves-effect waves-light m-b-5"
                    v-on:click="redirectLogin()"
                  >
                    Add to library
                  </button>
                </span>
                <span v-if="isLoggedIn">
                  <span v-if="!isInLibrary">
                    <button
                      class="btn btn-success btn-custom waves-effect waves-light m-b-5"
                      v-on:click="addLibrary(game)"
                    >
                      Add to library
                    </button>
                  </span>
                </span>
              </li>
              <span v-if="isInLibrary">
                <form v-on:submit.prevent="updateEntry(entry)">
                  <h5 class="font-weight-normal">My Rating</h5>
                  <p v-if="!onEdit">
                    {{ entry.rating }}
                  </p>
                  <select v-if="onEdit" class="browser-default custom-select mb-4" v-model="entry.rating">
                    <option value="" disabled>Your rating // {{ entry.rating }}</option>
                    <option value=""></option>
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
                  <h5 class="font-weight-normal">My Progress</h5>
                  <p v-if="!onEdit">
                    {{ entry.progress }}
                  </p>
                  <select v-if="onEdit" class="browser-default custom-select mb-4" v-model="entry.progress">
                    <option value="" disabled>{{ entry.progress }}</option>
                    <option value=""></option>
                    <option value="Playing">Playing</option>
                    <option value="Dropped">Dropped</option>
                    <option value="Backlog">Backlog</option>
                    <option value="Pre-ordered">Pre-ordered</option>
                    <option value="Completed">Completed</option>
                    <option value="100%">100%</option>
                  </select>
                  <div>
                    <button
                      v-if="!onEdit"
                      class="btn btn-success btn-custom waves-effect waves-light m-b-5"
                      v-on:click="editEntry(entry)"
                    >
                      Edit
                    </button>
                    <button
                      v-if="!onEdit"
                      class="btn btn-success btn-custom waves-effect waves-light m-b-5"
                      v-on:click="destroyEntry(entry)"
                    >
                      Remove
                    </button>
                    <button
                      v-if="onEdit"
                      class="btn btn-success btn-custom waves-effect waves-light m-b-5"
                      v-on:click="editStop(entry)"
                    >
                      Don't Save
                    </button>
                    <button
                      v-if="onEdit"
                      class="btn btn-success btn-custom waves-effect waves-light m-b-5"
                      v-on:click="updateEntry(entry)"
                    >
                      Update
                    </button>
                  </div>
                </form>
              </span>
            </ul>
          </div>
        </div>
        <div class="col-lg-7 col-md-6 pl-xl-3">
          <h5 class="font-weight-normal">Description</h5>
          <p>
            {{ game.description_raw }}
          </p>
          <span v-if="isInLibrary">
            <form v-on:submit.prevent="updateEntry(entry)">
              <h5 class="font-weight-normal">My Review</h5>
              <p v-if="!onEdit">
                {{ entry.review }}
              </p>
              <div class="form-group" v-if="onEdit">
                <textarea
                  v-model="entry.review"
                  class="form-control rounded-0"
                  id="exampleFormControlTextarea2"
                  rows="3"
                  placeholder="Review"
                ></textarea>
              </div>
              <h5 class="font-weight-normal">Other thoughts/notes</h5>
              <p v-if="!onEdit">
                {{ entry.note }}
              </p>
              <div class="form-group" v-if="onEdit">
                <textarea
                  v-model="entry.note"
                  class="form-control rounded-0"
                  id="exampleFormControlTextarea2"
                  rows="3"
                  placeholder="Note"
                ></textarea>
              </div>
            </form>
          </span>
        </div>
      </div>
    </div>
  </div>
</template>
<style>
body {
  color: #888888;
  margin-top: 20px;
}
.progress {
  position: relative;
  overflow: inherit;
  height: 6px;
  margin: 30px 0px 15px;
  width: 100%;
  display: inline-block;
  border-radius: 10px;
}

.progress .progress-bar {
  height: 6px;
  background: #009b72;
  border-radius: 10px;
}

.progress .progress-bar-title {
  position: absolute;
  left: 0;
  top: -30px;
  color: #818181;
  font-size: 16px;
}

.progress .progress-bar-number {
  position: absolute;
  right: 0;
  color: #888888;
  top: -30px;
  font-weight: 600;
  font-size: 14px;
}
.media {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-align: start;
  -ms-flex-align: start;
  align-items: flex-start;
}

.media-body {
  -webkit-box-flex: 1;
  -ms-flex: 1;
  flex: 1;
}

.text-black {
  color: #000000;
}

.font-weight-normal {
  font-weight: 500 !important;
}
.w-25 {
  width: 25% !important;
}
.text-muted {
  color: #b2b2b2 !important;
}
.mr-1,
.mx-1 {
  margin-right: 0.625rem !important;
}
</style>
