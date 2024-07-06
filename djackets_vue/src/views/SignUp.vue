<template>
  <div class="page-sign-up">
    <div class="row justify-content-center">
      <div class="col-md-4">
        <h1 class="display-4">Sign up</h1>

        <form @submit.prevent="submitForm">
          <div class="mb-3">
            <label for="username" class="form-label">Username</label>
            <input type="text" class="form-control" id="username" v-model="username" />
          </div>

          <div class="mb-3">
            <label for="password" class="form-label">Password</label>
            <input type="password" class="form-control" id="password" v-model="password" />
          </div>

          <div class="mb-3">
            <label for="password2" class="form-label">Repeat password</label>
            <input type="password" class="form-control" id="password2" v-model="password2" />
          </div>

          <div class="alert alert-danger" v-if="errors.length">
            <p v-for="error in errors" :key="error">{{ error }}</p>
          </div>

          <div class="mb-3">
            <button type="submit" class="btn btn-dark">Sign up</button>
          </div>

          <hr />

          Or <router-link to="/log-in">click here</router-link> to log in!
        </form>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from "axios";

  export default {
    name: "SignUp",
    data() {
      return {
        username: "",
        password: "",
        password2: "",
        errors: [],
      };
    },
    methods: {
      submitForm() {
        this.errors = [];

        if (this.username === "") {
          this.errors.push("The username is missing");
        }

        if (this.password === "") {
          this.errors.push("The password is too short");
        }

        if (this.password !== this.password2) {
          this.errors.push("The passwords doesn't match");
        }

        if (!this.errors.length) {
          const formData = {
            username: this.username,
            password: this.password,
          };

          axios
            .post("/api/v1/users/", formData)
            .then((response) => {
              toast({
                message: "Account created, please log in!",
                type: "is-success",
                dismissible: true,
                pauseOnHover: true,
                duration: 2000,
                position: "bottom-right",
              });

              this.$router.push("/log-in");
            })
            .catch((error) => {
              if (error.response) {
                for (const property in error.response.data) {
                  this.errors.push(`${property}: ${error.response.data[property]}`);
                }

                console.log(JSON.stringify(error.response.data));
              } else if (error.message) {
                this.errors.push("Something went wrong. Please try again");

                console.log(JSON.stringify(error));
              }
            });
        }
      },
    },
  };
</script>
