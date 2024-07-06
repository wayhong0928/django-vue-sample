<template>
  <div class="page-search">
    <div class="row">
      <div class="col-12">
        <h1 class="display-4">Search</h1>
        <h2 class="h5 text-muted">Search term: "{{ query }}"</h2>
      </div>
      <ProductBox
        v-for="product in products"
        :key="product.id"
        :product="product"
      />
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import ProductBox from '@/components/ProductBox.vue'

export default {
  name: 'Search',
  components: {
    ProductBox
  },
  data() {
    return {
      products: [],
      query: ''
    }
  },
  mounted() {
    document.title = 'Search | Djackets'
    let uri = window.location.search.substring(1)
    let params = new URLSearchParams(uri)

    if (params.get('query')) {
      this.query = params.get('query')
      this.performSearch()
    }
  },
  methods: {
    async performSearch() {
      this.$store.commit('setIsLoading', true)

      await axios
        .post('/api/v1/products/search', {'query': this.query})
        .then(response => {
          this.products = response.data
        })
        .catch(error => {
          console.log(error)
        })

      this.$store.commit('setIsLoading', false)
    }
  },
}
</script>