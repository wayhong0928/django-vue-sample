<template>
  <div class="col-md-3">
    <div class="card mb-4">
      <img :src="product.get_thumbnail" class="card-img-top">
      <div class="card-body">
        <h5 class="card-title">{{ product.name }}</h5>
        <p class="card-text text-muted">${{ product.price }}</p>
        <RouterLink :to="product.get_absolute_url" class="btn btn-dark mt-4">View details</RouterLink>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProductBox',
  props: {
    product: Object
  }
}
</script>

<style>
.image {
  margin-top: -1.25rem;
  margin-left: -1.25rem;
  margin-right: -1.25rem;
}
</style>