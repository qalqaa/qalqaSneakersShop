<script setup>
defineProps({
  title: String,
  price: Number
})
</script>

<template>
  <div class="product">
    <h2>Product: {{ title }}</h2>
    <p>Price: {{ price }}</p>
    <p>Added: {{ isAdded }}</p>
  </div>
</template>

<style scoped>
.product {
  border: 1px solid blue;
}
</style>
