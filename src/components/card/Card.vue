<script setup>
import { inject } from 'vue'
import { useRouter } from 'vue-router'
import { useStore } from 'vuex'

const props = defineProps({
  id: Number,
  imgUrl: String,
  title: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean
})

const router = useRouter()
const store = useStore()

const isAuth = inject('isAuth')

const goToDetails = () => {
  router.push({ name: 'CardOpened', params: { id: props.id } })
  store.commit('setFavorite', props.isFavorite)
}
</script>

<template>
  <div
    @click="goToDetails"
    class="relative card-hitbox bg-color-soft p-10 rounded-xl p-8 transition hover:-translate-y-1 inner-shadow"
  >
    <img
      v-if="isAuth"
      :src="isFavorite ? '/like-2.svg' : '/like-1.svg'"
      alt="likeButton"
      class="absolute cursor-pointer z-10 top-8 left-8"
    />
    <img class="transition card-lightning" :src="imgUrl" alt="Sneaker" />
    <p class="mb-5">{{ title }}</p>
    <div class="flex justify-between">
      <div class="flex flex-col">
        <span>Цена:</span>
        <b class="c-accent">{{ price }} руб.</b>
      </div>
    </div>
  </div>
</template>
