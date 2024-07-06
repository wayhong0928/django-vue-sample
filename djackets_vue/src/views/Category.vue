<template>
  <div class="page-category">
    <div class="row">
      <div class="col-12">
        <h2 class="display-4 text-center">{{ category.name }}</h2>
      </div>
      <ProductBox
        v-for="product in category.products"
        :key="product.id"
        :product="product"
      />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import ProductBox from '@/components/ProductBox';

export default {
  name: 'Category',
  components: {
    ProductBox
  },
  data() {
    return {
      category: {
        products: []
      }
    }
  },
  mounted() {
    this.getCategory()
  },
  watch: {
    $route(to, from) {
      if (to.name === 'Category') {
        this.getCategory()
      }
    }
  },
  methods: {
    async getCategory() {
      const categorySlug = this.$route.params.category_slug;

      this.$store.commit('setIsLoading', true);

      axios
        .get(`/api/v1/products/${categorySlug}/`)
        .then(response => {
          this.category = response.data;
          document.title = this.category.name + ' | Djackets';
        })
        .catch(error => {
          console.log(error.response.data);
          this.$toast.show('Something went wrong. Please try again', {
            type: 'danger',
            duration: 2000,
            position: 'bottom-right',
            dismissible: true,
            pauseOnHover: true,
          });
        });
      this.$store.commit('setIsLoading', false);
    }
  }
}
</script>

<style>

</style>
