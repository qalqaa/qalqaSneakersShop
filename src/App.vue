<script setup>
import { provide, ref, watch, computed } from 'vue'

import Header from './components/Header.vue'
import Drawer from './components/Drawer.vue'

/*Корзина*/
const cart = ref([])


const cartOpen = ref(false)

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))
const vatPrice = computed(() => totalPrice.value * 0.2)



const closeCart = () => {
  cartOpen.value = false
}
const openCart = () => {
  cartOpen.value = true
}

function addToCart(item) {
  cart.value.push(item)
  item.isAdded = true
}

function removeFromCart(item) {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}



watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  { deep: true }
)

provide('cart', { cart, closeCart, openCart, addToCart, removeFromCart })

/*Корзина*/
</script>

<template>
  <Drawer
    v-if="cartOpen"
    :total-price="totalPrice"
    :vat-price="vatPrice"
  />
  <div class="bg-color-mute w-4/5 m-auto rounded-xl shadow-xl mt-10">
    <Header :total-price="totalPrice" @open-cart="openCart" />
    <div class="p-10">
      <RouterView />
    </div>
  </div>
</template>

<style scoped></style>
