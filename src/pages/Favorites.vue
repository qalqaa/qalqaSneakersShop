<script setup>
import { onMounted, ref } from 'vue'
import axios from 'axios'

import List from '../components/List.vue'

const favorites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get(
      'https://4c860bad2146c5b3.mokky.dev/favorites?_relations=items'
    )

    favorites.value = data.map((obj) => obj.item)
  } catch (error) {
    console.error(error)
  }
})
</script>


<template>
  <h2 class="text-3xl font-bold mb-8">Мои закладки</h2>

  <List :items="favorites" is-favorites />
</template>