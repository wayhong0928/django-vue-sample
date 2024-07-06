<template>
  <div class="home">
    <section class="jumbotron jumbotron-fluid bg-light mb-6">
      <div class="container text-center">
        <p class="display-4 mb-6">
          Welcome to Djacket
        </p>
        <p class="lead">
          The best jacket store online
        </p>
      </div>
    </section>
    <div class="row">
      <div class="col-12">
        <h2 class="display-4 text-center">Latest products</h2>
      </div>
      <ProductBox
        v-for="product in latestProducts"
        :key="product.id"
        :product="product"
      />
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import ProductBox from '@/components/ProductBox'

export default {
  name: 'HomeView',
  data() {
    return {
      latestProducts : []
    }
  },
  components: {
    ProductBox
  },
  mounted() {
    this.getLatestProducts()
    document.title = 'Home | Djackets'
  },
  methods: {
    async getLatestProducts() {
      this.$store.commit('setIsLoading', true)

      await axios
        .get('/api/v1/latest-products/')
        .then(response => {
          this.latestProducts = response.data
        })
        .catch(error => {
          console.log(error)
        })

      this.$store.commit('setIsLoading', false)
    }
  }
}
</script>

