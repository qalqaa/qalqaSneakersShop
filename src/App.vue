<script setup>
import { ref } from 'vue'

let count = ref(0)

function increment() {
  count.value++
  console.log(count)
}

// function updateCount(event) {
//   count.value = Number(event.target.value)
// }
</script>

<template>
  <h1>{{ count }}</h1>
  <button @click="increment">+</button>
  <input @keyup.ctrl.enter="increment" />
</template>

<style scoped></style>
