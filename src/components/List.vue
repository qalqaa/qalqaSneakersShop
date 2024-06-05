<script setup>
import Card from './Card.vue';

defineProps({
    items: Array
})

const emit = defineEmits(['addToFavorite', 'addToCart'])

</script>
<template>
    <div class="grid grid-cols-3 gap-5">
        <Card 
        v-for="item in items" 
        :key="item.id"
        :id="item.id"
        :title="item.title" 
        :img-url="item.imageUrl"
        :price="item.price"
        :onClickFavorite="() => emit('addToFavorite', item) "
        :onClickAdd="() => emit('addToCart', item)"
        :isFavorite="item.isFavorite"
        :isAdded="item.isAdded"
        />
    </div>
</template>