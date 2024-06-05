<script setup>
import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'

const emit = defineEmits(['createOrder'])

defineProps({
  totalPrice: Number,
  vatPrice: Number
})
</script>

<template>
  <div class="fixed top-0 left-0 w-full h-full bg-black/70 z-10"></div>
  <div class="bg-color-mute w-96 h-full fixed right-0 top-0 z-20 p-10">
    <DrawerHead />
    <CartItemList />
    <div class="flex flex-col gap-5 mt-7 mb-6">
      <div class="flex gap-2">
        <span>Итого: </span>
        <div class="flex-1 border-zinc-400 border-b border-dashed"></div>
        <b>{{ totalPrice }} ₽</b>
      </div>
      <div class="flex gap-2">
        <span>НДС 20%: </span>
        <div class="flex-1 border-zinc-400 border-b border-dashed"></div>
        <b>{{ vatPrice }} ₽</b>
      </div>
      <button
        class="mt-4 w-full font-bold rounded-xl p-3 hover-accent-shadow-box active:text-white disabled:opacity-50 disabled:shadow-none disabled:cursor-not-allowed transition cursor-pointer"
        @click="emit('createOrder')"
        :disabled="totalPrice ? false : true"
      >
        Оформить заказ
      </button>
    </div>
  </div>
</template>