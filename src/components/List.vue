<script setup>import Card from './Card.vue';
const onClickAdd = () => { alert(111) }
</script>
<template>
    <div class="grid grid-cols-3 gap-5">
        <Card :onClickAdd="onClickAdd" :is-favorite="true" :is-added="true" :price="5000"
            title="Кроссовки Nike Air Jordan 1" img-url="/sneakers/sneakers-1.png" />
    </div>
</template>