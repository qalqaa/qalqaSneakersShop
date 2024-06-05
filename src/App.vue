<script setup>
import Header from './components/Header.vue';
import List from './components/List.vue';
import Drawer from './components/Drawer.vue';
</script>

<template>
  <!-- <Drawer /> -->
  <div class="bg-color-mute w-4/5 m-auto rounded-xl shadow-xl mt-10">
    <Header />
    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>
        <div class="flex gap-4">
          <select class="py-2 px-4 border border-zinc-500 rounded-md">
            <option>По названию</option>
            <option>По цене (возрастанию)</option>
            <option>По названию (убыванию)</option>
          </select>
          <div class="relative">
            <img class="absolute top-3 left-4" src="/search.svg" alt="search">
            <input
              class="p-2 border border-zinc-500 pl-11 pr-4 transition outline-none focus:border-gray-300 rounded-md"
              type="text" placeholder="Поиск...">
          </div>
        </div>
      </div>
      <List />
    </div>
  </div>
</template>

<style scoped></style>
