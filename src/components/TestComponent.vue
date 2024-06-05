<script setup>
const props = defineProps({
  title: String,
  price: Number,
  isAdded: Boolean
})
console.log(props)

</script>

<template>
  <div class="product">
    <h2>Product: {{ title }}</h2>
    <p>Price: {{ price }}</p>
    <p>Added: {{ isAdded }}</p>
    <slot></slot>
  </div>
</template>

<style scoped>
.product {
  border: 1px solid blue;
}
</style>
