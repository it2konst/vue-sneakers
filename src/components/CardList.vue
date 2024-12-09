<script setup>
import Card from './Card.vue'

defineProps({
    items: Array,
    isFavorites: Boolean,
})

const emit = defineEmits(['addToFavorite', 'addToCart'])
</script>

<template>
    <div class="flex flex-wrap justify-center gap-5" v-auto-animate>
        <Card
            v-for="item in items"
            :key="item.id"
            :id="item.id"
            :title="item.title"
            :imageUrl="item.imgUrl"
            :price="item.price"
            :onClickFavorite="isFavorites ? null : () => emit('addToFavorite', item)"
            :onClickAdd="isFavorites ? null : () => emit('addToCart', item)"
            :isFavorite="item.isFavorite"
            :isAdded="item.isAdded"
        />
    </div>
</template>
