<script setup>
defineProps({
    title: String,
    price: Number,
})
</script>


<template>
    <div class="flex items-center border border-zinc-500 rounded-xl p-4 gap-4">
        <img class="w-20 h-15" src="/sneakers/sneakers-1.png" alt="Sneakers">

        <div class="flex flex-col">
            <p>{{ title }}Кросовки хуйниnnnnnnnnnnnnnn</p>

            <div class="flex justify-between">
                <b class="c-accent">1111111{{ price }} руб.</b>
                <img class="opacity-30 cursor-pointer transition hover:opacity-100" src="/close.svg" alt="close">
            </div>
        </div>
    </div>
</template>