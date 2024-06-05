<script setup>import Card from './Card.vue';</script>
<template>
    <div class="p-10">
        <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>

        <div class="grid grid-cols-3 gap-5">
            <Card :is-favorite="true" :is-added="true" :price="5000" title="Кроссовки Nike Air Jordan 1" img-url="/sneakers/sneakers-1.png" />
            <Card />
            <Card />
            <Card />
            <Card />
            <Card />
        </div>
    </div>
</template>