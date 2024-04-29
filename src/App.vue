<script>
export default {
  data() {
    return {
      count: 0
    }
  },
  methods: {
    increment() {
      this.count++
    }
  }
}
</script>

<template>
  <h1>{{ count }}</h1>
  <button @click="increment">+</button>
</template>

<style scoped></style>
