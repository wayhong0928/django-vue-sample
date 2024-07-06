<template>
  <div class="page-my-account">
    <div class="row">
      <div class="col-12">
        <h1 class="display-4">My Account</h1>
      </div>

      <div class="col-12">
        <button @click="logout" class="btn btn-danger">Log out</button>
      </div>
    </div>
  </div>
</template>


<script>
  import axios from "axios";
  export default {
    name: "MyAccount",
    methods: {
      logout() {
        axios.defaults.headers.common["Authorization"] = "";

        localStorage.removeItem("token");
        localStorage.removeItem("username");
        localStorage.removeItem("userid");

        this.$store.commit("removeToken");
        this.$router.push("/");
      },
    },
  };
</script>
