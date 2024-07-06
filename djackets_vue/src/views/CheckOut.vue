<template>
  <div class="page-checkout">
    <div class="row">
      <div class="col-12">
        <h1 class="display-4">Checkout</h1>
      </div>
      <div class="col-12">
        <div class="card">
          <div class="card-body">
            <table class="table table-striped">
              <thead>
                <tr>
                  <th>Product</th>
                  <th>Price</th>
                  <th>Quantity</th>
                  <th>Total</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="item in cart.items" :key="item.product.id">
                  <td>{{ item.product.name }}</td>
                  <td>${{ item.product.price }}</td>
                  <td>{{ item.quantity }}</td>
                  <td>${{ getItemTotal(item).toFixed(2) }}</td>
                </tr>
              </tbody>
              <tfoot>
                <tr>
                  <td colspan="2">Total</td>
                  <td>{{ cartTotalLength }}</td>
                  <td>${{ cartTotalPrice.toFixed(2) }}</td>
                </tr>
              </tfoot>
            </table>
          </div>
        </div>
      </div>
      <div class="col-12 mt-3">
        <div class="card">
          <div class="card-body">
            <h2 class="h5">Shipping details</h2>
            <p class="text-muted mb-4">*All fields are required</p>
            <div class="row">
              <div class="col-md-6">
                <div class="mb-3">
                  <label>First name*</label>
                  <input type="text" class="form-control" v-model="first_name" />
                </div>
                <div class="mb-3">
                  <label>Last name*</label>
                  <input type="text" class="form-control" v-model="last_name" />
                </div>
                <div class="mb-3">
                  <label>Email*</label>
                  <input type="email" class="form-control" v-model="email" />
                </div>
                <div class="mb-3">
                  <label>Phone*</label>
                  <input type="text" class="form-control" v-model="phone" />
                </div>
              </div>
              <div class="col-md-6">
                <div class="mb-3">
                  <label>Address*</label>
                  <input type="text" class="form-control" v-model="address" />
                </div>
                <div class="mb-3">
                  <label>Zip code*</label>
                  <input type="text" class="form-control" v-model="zipcode" />
                </div>
                <div class="mb-3">
                  <label>Place*</label>
                  <input type="text" class="form-control" v-model="place" />
                </div>
              </div>
            </div>
            <div class="alert alert-danger mt-4" v-if="errors.length">
              <p v-for="error in errors" :key="error">{{ error }}</p>
            </div>
            <hr />
            <div class="mb-5" id="card-element"></div>
            <template v-if="cartTotalLength">
              <hr />
              <button class="btn btn-dark" @click="submitForm">Pay with Stripe</button>
            </template>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from "axios";
  export default {
    name: "Checkout",
    data() {
      return {
        cart: {
          items: [],
        },
        stripe: {},
        card: {},
        first_name: "",
        last_name: "",
        email: "",
        phone: "",
        address: "",
        zipcode: "",
        place: "",
        errors: []
      };
    },
    mounted() {
      document.title = "Checkout | Djackets";
      this.cart = this.$store.state.cart;
    },
    methods: {
      getItemTotal(item) {
        return item.quantity * item.product.price;
      },
      submitForm() {
        this.errors = [];
        if (this.first_name === "") {
          this.errors.push("The First name field is missing!");
        }
        if (this.last_name === "") {
          this.errors.push("The Last name field is missing!");
        }
        if (this.email === "") {
          this.errors.push("The Email field is missing!");
        }
        if (this.phone === "") {
          this.errors.push("The Phone field is missing!");
        }
        if (this.address === "") {
          this.errors.push("The Address field is missing!");
        }
        if (this.zipcode === "") {
          this.errors.push("The Zipcode field is missing!");
        }
        if (this.place === "") {
          this.errors.push("The Place field is missing!");
        }
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

<style>

</style>
