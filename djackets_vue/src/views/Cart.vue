<template>
  <div class="page-cart">
    <div class="row">
      <div class="col-12">
        <h1 class="display-4">Cart</h1>
      </div>
      <div class="col-12">
        <div class="card">
          <div class="card-body">
            <table class="table table-striped" v-if="cartTotalLength">
              <thead>
                <tr>
                  <th>Product</th>
                  <th>Price</th>
                  <th>Quantity</th>
                  <th>Total</th>
                  <th></th>
                </tr>
              </thead>
              <tbody>
                <cartItem
                  v-for="item in cart.items"
                  :key="item.product.id"
                  :initialItem="item"
                  @removeFromCart="removeFromCart"
                />
              </tbody>
            </table>
            <p v-else>You don't have any products in your cart...</p>
          </div>
        </div>
      </div>
      <div class="col-12 mt-3">
        <div class="card">
          <div class="card-body">
            <h2 class="h5">Summary</h2>
            <strong>${{ cartTotalPrice.toFixed(2) }}</strong>, {{ cartTotalLength }} items
            <hr />
            <router-link to="/cart/checkout" class="btn btn-dark">Proceed to checkout</router-link>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from "axios";
  import CartItem from "@/components/CartItems.vue";
  export default {
    name: "Cart",
    components: {
      CartItem,
    },
    data() {
      return {
        cart: {
          items: [],
        },
      };
    },
    mounted() {
      this.cart = this.$store.state.cart;
    },
    methods: {
      removeFromCart(item) {
        this.cart.items = this.cart.items.filter((i) => i.product.id !== item.product.id);
      },
    },
    computed: {
      cartTotalLength() {
        return this.cart.items.reduce((acc, curVal) => {
          return (acc += curVal.quantity);
        }, 0);
      },
      cartTotalPrice() {
        return this.cart.items.reduce((acc, curVal) => {
          return (acc += curVal.product.price * curVal.quantity);
        }, 0);
      },
    },
  };
</script>
