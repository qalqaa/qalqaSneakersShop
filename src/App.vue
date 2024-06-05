<script setup>
import { onMounted, provide, reactive, ref, watch, computed } from 'vue'
import axios from 'axios'

import Header from './components/Header.vue'
import List from './components/List.vue'
import Drawer from './components/Drawer.vue'

const items = ref([])
const cart = ref([])
const isLoading = ref(false)

const cartOpen = ref(false)

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))
const vatPrice = computed(() => totalPrice.value * 0.2)

const cartButtonDisabled = computed(() =>
  isLoading.value ? true : totalPrice.value ? false : true
)

const closeCart = () => {
  cartOpen.value = false
}
const openCart = () => {
  cartOpen.value = true
}

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

function addToCart(item) {
  cart.value.push(item)
  item.isAdded = true
}

function removeFromCart(item) {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

const createOrder = async () => {
  try {
    isLoading.value = true
    const { data } = await axios.post(`https://4c860bad2146c5b3.mokky.dev/orders`, {
      items: cart.value,
      totalPrice: totalPrice.value
    })
    cart.value = []

    return data
  } catch (error) {
    console.error(error)
  } finally {
    isLoading.value = false
  }
}

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }

  console.log(cart)
}

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(`https://4c860bad2146c5b3.mokky.dev/favorites`)

    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.productId === item.id)

      if (!favorite) {
        return item
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (error) {
    console.error(error)
  }
}

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        productId: item.id
      }

      item.isFavorite = true

      const { data } = await axios.post(`https://4c860bad2146c5b3.mokky.dev/favorites`, obj)

      item.favoriteId = data.id

      console.log(data)
    } else {
      item.isFavorite = false

      await axios.delete(`https://4c860bad2146c5b3.mokky.dev/favorites/${item.favoriteId}`)

      item.favoriteId = null
    }
  } catch (err) {
    console.error(err)
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get(`https://4c860bad2146c5b3.mokky.dev/items`, {
      params
    })

    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      isAdded: false
    }))
  } catch (error) {
    console.error(error)
  }
}

onMounted(async () => {
  const localCart = [localStorage.getItem('cart')]
  cart.value = localCart ? JSON.parse(localCart) : []

  
  await fetchItems()
  await fetchFavorites()
  
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem)=> cartItem.id === item.id)
  }))
})

watch(filters, fetchItems)

watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})

watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  { deep: true }
)

provide('cart', { cart, closeCart, openCart, addToCart, removeFromCart })
</script>

<template>
  <Drawer
    v-if="cartOpen"
    :total-price="totalPrice"
    :vat-price="vatPrice"
    @create-order="createOrder"
    :button-disabled="cartButtonDisabled"
  />
  <div class="bg-color-mute w-4/5 m-auto rounded-xl shadow-xl mt-10">
    <Header :total-price="totalPrice" @open-cart="openCart" />
    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>
        <div class="flex gap-4">
          <select @change="onChangeSelect" class="py-2 px-4 border border-zinc-500 rounded-md">
            <option value="title">По названию</option>
            <option value="price">По цене (возрастанию)</option>
            <option value="-price">По цене (убыванию)</option>
          </select>
          <div class="relative">
            <img class="absolute top-3 left-4" src="/search.svg" alt="search" />
            <input
              @input="onChangeSearchInput"
              class="p-2 border border-zinc-500 pl-11 pr-4 transition outline-none focus:border-gray-300 rounded-md"
              type="text"
              placeholder="Поиск..."
            />
          </div>
        </div>
      </div>
      <List :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus" />
    </div>
  </div>
</template>

<style scoped></style>
