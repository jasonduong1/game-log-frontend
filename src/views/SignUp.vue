<script>
import axios from "axios";

export default {
  data: function () {
    return {
      newUserParams: {},
      errors: [],
    };
  },
  methods: {
    submit: function () {
      axios
        .post("/users", this.newUserParams)
        .then((response) => {
          console.log(response.data);
          this.$router.push("/login");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>

<template>
  <div class="app">
    <div id="hero" class="hero route bg-image" style="background-image: url(assets/img/hero-bg.jpg)">
      <div class="overlay-itro"></div>
      <div class="hero-content display-table">
        <div class="container px-4 py-5 px-md-5 text-center text-lg-start my-5">
          <div class="row gx-lg-5 align-items-center mb-5">
            <div class="col-lg-3 mb- mb-lg-0" style="z-index: 10"></div>

            <div class="col-lg-6 mb-5 mb-lg-0 position-relative">
              <div id="radius-shape-1" class="position-absolute rounded-circle shadow-5-strong"></div>
              <div id="radius-shape-2" class="position-absolute shadow-5-strong"></div>

              <div class="card bg-glass">
                <div class="card-body px-4 py-4 px-md-5">
                  <h1 class="hero-title mb-4">Signup.</h1>
                  <form v-on:submit.prevent="submit()">
                    <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
                    <!-- Username input -->
                    <div class="form-outline mb-2">
                      <input type="text" id="form3Example2" class="form-control" v-model="newUserParams.user_name" />
                      <label class="form-label" for="form3Example2">Username</label>
                    </div>
                    <!-- Name input -->
                    <div class="form-outline mb-2">
                      <input type="text" id="form3Example1" class="form-control" v-model="newUserParams.name" />
                      <label class="form-label" for="form3Example1">Name</label>
                    </div>
                    <!-- Email input -->
                    <div class="form-outline mb-2">
                      <input type="email" id="form3Example3" class="form-control" v-model="newUserParams.email" />
                      <label class="form-label" for="form3Example3">Email address</label>
                    </div>

                    <!-- Password input -->
                    <div class="form-outline mb-2">
                      <input type="password" id="form3Example4" class="form-control" v-model="newUserParams.password" />
                      <label class="form-label" for="form3Example4">Password</label>
                    </div>

                    <!-- Password confirmation -->
                    <div class="form-outline mb-4">
                      <input
                        type="password"
                        id="form3Example4"
                        class="form-control"
                        v-model="newUserParams.password_confirmation"
                      />
                      <label class="form-label" for="form3Example4">Password confirmation</label>
                    </div>

                    <!-- Submit button -->
                    <button type="submit" class="btn btn-success btn-custom waves-effect waves-light m-b-5">
                      Sign up
                    </button>
                  </form>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- <form v-on:submit.prevent="submit()">
      <h1>Signup</h1>
      <ul>
        <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
      </ul>
      <div>
        <label>Username:</label>
        <input type="text" v-model="newUserParams.user_name" />
      </div>
      <div>
        <label>Name:</label>
        <input type="text" v-model="newUserParams.name" />
      </div>
      <div>
        <label>Email:</label>
        <input type="email" v-model="newUserParams.email" />
      </div>
      <div>
        <label>Password:</label>
        <input type="password" v-model="newUserParams.password" />
      </div>
      <div>
        <label>Password confirmation:</label>
        <input type="password" v-model="newUserParams.password_confirmation" />
      </div>
      <input type="submit" value="Submit" />
    </form> -->
  </div>
</template>
<style>
.background-radial-gradient {
  background-color: hsl(219, 67%, 44%);
  background-image: radial-gradient(
      650px circle at 0% 0%,
      hsl(218, 41%, 35%) 15%,
      hsl(218, 41%, 30%) 35%,
      hsl(218, 41%, 20%) 75%,
      hsl(218, 41%, 19%) 80%,
      transparent 100%
    ),
    radial-gradient(
      1250px circle at 100% 100%,
      hsl(218, 41%, 45%) 15%,
      hsl(218, 41%, 30%) 35%,
      hsl(218, 41%, 20%) 75%,
      hsl(218, 41%, 19%) 80%,
      transparent 100%
    );
}

#radius-shape-1 {
  height: 220px;
  width: 220px;
  top: -60px;
  left: -130px;
  background: radial-gradient(#44006b, #ad1fff);
  overflow: hidden;
}

#radius-shape-2 {
  border-radius: 38% 62% 63% 37% / 70% 33% 67% 30%;
  bottom: -60px;
  right: -110px;
  width: 300px;
  height: 300px;
  background: radial-gradient(#44006b, #ad1fff);
  overflow: hidden;
}

.bg-glass {
  background-color: hsla(268, 68%, 17%, 0.332) !important;
  backdrop-filter: saturate(200%) blur(25px);
}
</style>
