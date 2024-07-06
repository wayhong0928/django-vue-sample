<template>
  <div class="page-log-in">
    <div class="row justify-content-center">
      <div class="col-md-4">
        <h1 class="display-4">Log in</h1>

        <form @submit.prevent="submitForm">
          <div class="mb-3">
            <label for="username" class="form-label">Username</label>
            <input type="text" class="form-control" id="username" v-model="username" />
          </div>

          <div class="mb-3">
            <label for="password" class="form-label">Password</label>
            <input type="password" class="form-control" id="password" v-model="password" />
          </div>

          <div class="alert alert-danger" v-if="errors.length">
            <p v-for="error in errors" :key="error">{{ error }}</p>
          </div>

          <div class="mb-3">
            <button type="submit" class="btn btn-dark">Log in</button>
          </div>

          <hr />

          Or <router-link to="/sign-up">click here</router-link> to sign up!
        </form>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from "axios";

  export default {
    name: "LogIn",
    data() {
      return {
        username: "",
        password: "",
        errors: [],
      };
    },
    mounted() {
      document.title = "Log In | Djackets";
    },
    methods: {
      async submitForm() {
        axios.defaults.headers.common["Authorization"] = "";

        localStorage.removeItem("token");

        const formData = {
          username: this.username,
          password: this.password,
        };

        await axios
          .post("/api/v1/token/login/", formData)
          .then((response) => {
            const token = response.data.auth_token;

            this.$store.commit("setToken", token);

            axios.defaults.headers.common["Authorization"] = "Token " + token;

            localStorage.setItem("token", token);

            const toPath = this.$route.query.to || "/cart";

            this.$router.push(toPath);
          })
          .catch((error) => {
            if (error.response) {
              for (const property in error.response.data) {
                this.errors.push(`${property}: ${error.response.data[property]}`);
              }
            } else {
              this.errors.push("Something went wrong. Please try again");

              console.log(JSON.stringify(error));
            }
          });
      },
    },
  };
</script>
