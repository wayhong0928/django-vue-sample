<template>
  <div class="wrapper">
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <div class="container-fluid">
        <router-link to="/" class="navbar-brand"><strong>Djackets</strong></router-link>
        <button
          class="navbar-toggler"
          type="button"
          aria-label="Toggle navigation"
          @click="showMobileMenu = !showMobileMenu"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" v-bind:class="{ show: showMobileMenu }" id="navbar-menu">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <li class="nav-item">
              <form methods="get" action="/search" class="d-flex">
                <input type="text" class="form-control me-2" placeholder="What are you looking for?" name="query" />
                <button class="btn btn-success" type="submit">
                  <span class="icon">
                    <i class="fas fa-search">search!</i>
                  </span>
                </button>
              </form>
            </li>
          </ul>
          <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
            <li class="nav-item">
              <router-link to="/summer" class="nav-link">Summer</router-link>
            </li>
            <li class="nav-item">
              <router-link to="/winter" class="nav-link">Winter</router-link>
            </li>
            <li class="nav-item">
              <div class="d-flex align-items-center">
                <template v-if="$store.state.isAuthenticated">
                  <router-link to="/my-account" class="btn btn-outline-secondary me-2">My account</router-link>
                </template>
                <template v-else>
                  <router-link to="/log-in" class="btn btn-outline-secondary me-2">Log in</router-link>
                </template>
                <router-link to="/cart" class="btn btn-success">
                  <span class="icon"><i class="fas fa-shopping-cart"></i></span>
                  <span>Cart({{ cartTotalLength }})</span>
                </router-link>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </nav>

    <div class="text-center" v-bind:class="{ 'is-loading': $store.state.isLoading }">
      <div class="spinner-border" role="status" v-if="$store.state.isLoading">
        <span class="visually-hidden">Loading...</span>
      </div>
    </div>

    <section class="py-5">
      <router-view />
    </section>
    <footer class="footer bg-light text-dark py-4">
      <div class="container text-center">
        <p>Copyright (c) 2024</p>
      </div>
    </footer>
  </div>
</template>



<script>
  import axios from "axios";

  export default {
    data() {
      return {
        showMobileMenu: false,
        cart: {
          items: [],
        },
      };
    },
    beforeCreate() {
      this.$store.commit("initializeStore");
      const token = this.$store.state.token;

      if (token) {
        axios.defaults.headers.common["Authorization"] = "Token " + token;
      } else {
        axios.defaults.headers.common["Authorization"] = "";
      }
    },
    mounted() {
      this.cart = this.$store.state.cart;
    },
    computed: {
      cartTotalLength() {
        let totalLength = 0;
        for (let i = 0; i < this.cart.items.length; i++) {
          totalLength += this.cart.items[i].quantity;
        }
        return totalLength;
      },
    },
  };
</script>

<style lang="scss">
  .lds-dual-ring {
    display: inline-block;
    width: 80px;
    height: 80px;
  }
  .lds-dual-ring::after {
    content: " ";
    display: block;
    width: 64px;
    height: 64px;
    margin: 8px;
    border-radius: 50%;
    border: 6px solid #ccc;
    border-color: #ccc transparent #ccc transparent;
    animation: lds-dual-ring 1.2s linear infinite;
  }
  @keyframes lds-dual-ring {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }

  .is-loading-bar {
    height: 0;
    overflow: hidden;
    -webkit-transition: all 0.3s;
    transition: all 0.3s;
    &.is-loading {
      height: 80px;
    }
  }
</style>
