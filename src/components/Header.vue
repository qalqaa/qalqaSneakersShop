<script setup>
const emit = defineEmits(['openCart'])

defineProps({
  totalPrice: Number
})

</script>


<template>
  <header class="flex justify-between border-b border-zinc-500 px-8 py-3">
    <div class="flex items-center">
      <img src="/logo.png" alt="Logo" class="w-10 mr-4" />
      <div>
        <h2 class="c-accent accent-shadow-text text-xl uppercase">qalqa Sneakers</h2>
        <p>Магазин лучших кросовок</p>
      </div>
    </div>
    <ul class="flex items-center gap-5">
      <li
        @click="() => emit('openCart')"
        class="flex items-center gap-3 cursor-pointer hover-accent"
      >
        <img src="/cart.svg" alt="Cart" />
        <b>{{ totalPrice }} ₽</b>
      </li>
      <li class="flex items-center gap-3 cursor-pointer hover-accent">
        <img src=" /heart.svg" alt="Cart" />
        <span>Закладки</span>
      </li>
      <li class="flex items-center gap-3 cursor-pointer hover-accent">
        <img src=" /profile.svg" alt="Cart" />
        <span>Профиль</span>
      </li>
    </ul>
  </header>
</template>