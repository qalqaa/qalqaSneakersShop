<script setup>
defineProps({
    imgUrl: String,
    title: String,
    price: Number,
    isFavorite: Boolean,
    isAdded: Boolean
})
</script>

<template>
    <div>
        <div
            class="relative border border-zinc-500 p-10 rounded-xl p-8 transition hover:-translate-y-2 hover-accent-shadow-box">
            <img :src="isFavorite ? '/like-2.svg' : '/like-1.svg'" alt="likeButton" class="absolute top-8 left-8">
            <img :src="imgUrl" alt="Sneaker">
            <p class="mb-5">{{ title }}</p>
            <div class="flex justify-between">
                <div class="flex flex-col">
                    <span>Цена:</span>
                    <b class="c-accent">{{ price }} руб.</b>
                </div>

                <img :src="isAdded ? '/checked.svg' : '/plus.svg'" alt="plus">
            </div>
        </div>
    </div>
</template>