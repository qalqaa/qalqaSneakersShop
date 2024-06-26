<script setup>
import { inject, reactive, watch, ref, onMounted } from 'vue'
import axios from 'axios'
import debounce from 'lodash.debounce'

import List from '../components/List.vue'

const { cart, addToCart, removeFromCart } = inject('cart')

const items = ref([])
const isEmpty = ref(false)

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

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

const onChangeSearchInput = debounce((event) => {
  filters.searchQuery = event.target.value
}, 500)

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        item_id: item.id
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

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(`https://4c860bad2146c5b3.mokky.dev/favorites`)

    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.item_id === item.id)

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

    if (data.length === 0) {
      isEmpty.value = true
    } else {
      isEmpty.value = false
    }

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
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
  }))
})

watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})

watch(filters, fetchItems)
</script>


<template>
  <div class="flex justify-between items-center">
    <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>
    <div class="w-1/3 flex gap-2">
      <select
        @change="onChangeSelect"
        class="w-1/2 py-2 px-4 outline-none bg-color-soft inner-shadow rounded-md"
      >
        <option value="title">По названию</option>
        <option value="price">По цене (возрастанию)</option>
        <option value="-price">По цене (убыванию)</option>
      </select>
      <div class="w-1/2 relative">
        <img class="absolute top-3 left-4" src="/search.svg" alt="search" />
        <input
          @input="onChangeSearchInput"
          class="p-2 bg-color-soft inner-shadow pl-11 pr-4 transition outline-none rounded-md"
          type="text"
          placeholder="Поиск..."
        />
      </div>
    </div>
  </div>
  <div v-if="!isEmpty">
    <List :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus" />
  </div>
  <div class="flex flex-col justify-center items-center" v-else>
    <img height="50" width="50" src="/emoji-1.png" alt="Sad emoji" />
    <p class="text-xl mt-3">По вашему запросу ничего не найдено</p>
  </div>
</template>