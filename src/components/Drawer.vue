<script setup>
import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'
import infoBlock from './infoBlock.vue'
const emit = defineEmits(['createOrder'])

defineProps({
  totalPrice: Number,
  vatPrice: Number,
  buttonDisabled: Boolean
})
</script>

<template>
  <div class="fixed top-0 left-0 w-full h-full bg-black/70 z-20"></div>
  <div class="bg-color-mute w-96 h-full fixed right-0 top-0 z-30 p-10">
    <DrawerHead />
    <div v-if="!totalPrice" class="flex h-full items-center">
      <infoBlock
        title="Корзина пуста"
        descr="Добавьте хотя бы одну пару кроссовок, чтобы оформить
    заказ."
        imgUrl="/package-icon.png"
      />
    </div>

    <div v-else>
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
          :disabled="buttonDisabled"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>