<template>
  <div class="page-product">
    <div class="row">
      <div class="col-md-9">
        <figure class="figure mb-6">
          <img :src="product.get_image" class="figure-img img-fluid">
        </figure>
        <h1 class="display-4">{{ product.name }}</h1>
        <p>{{ product.description }}</p>
      </div>

      <div class="col-md-3">
        <h2 class="h5">Information</h2>
        <p><strong>Price:</strong> {{ product.price }}</p>
        <div class="input-group mt-4">
          <input type="number" class="form-control" min="1" v-model="quantity">
          <button class="btn btn-dark" @click="addToCart">Add to cart</button>
        </div>
      </div>
    </div>    
  </div>
</template>


<script>
  import axios from "axios";

  export default {
    name: 'Product',
    data() {
      return {
        product: {},
        quantity: 1
      }
    },
    mounted() {
      this.getProduct()
    },
    methods: {
      async getProduct() {
        this.$store.commit('setIsLoading', true)

        const category_slug = this.$route.params.category_slug
        const product_slug = this.$route.params.product_slug

        await axios 
          .get(`/api/v1/products/${category_slug}/${product_slug}`)
          .then(response => {
            this.product = response.data
            document.title = this.product.name + ' | Djackets'
          })
          .catch(error => {
            console.log(error)
          })
        
        this.$store.commit('setIsLoading', false)
      },
      addToCart() {
        if (isNaN(this.quantity) || this.quantity < 1) {
          this.quantity = 1
        } 
        const item = {
          product: this.product,
          quantity: this.quantity
        }
        this.$store.commit('addToCart', item)

        toast({
          message: 'The product was added to the cart',
          type: 'is-success',
          dismissible: true,
          pauseOnHover: true,
          duration: 2000,
          position: 'bottom-right',
        })
      }
    }
  }
</script>

<style>

</style>