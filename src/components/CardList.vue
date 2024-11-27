<script setup>
// import { inject } from 'vue'
import Card from './Card.vue'

defineProps({
  items: Array,
})

const emit = defineEmits(['addToFavorite', 'addToCart'])

// const onClickAdd = () => {
//   emit('addToFavorite', item)
// }

// const addToFavorite = inject('addToFavorite')
</script>

<template>
  <!-- <div class="grid grid-cols-4 gap-5"> -->
  <div class="flex flex-wrap justify-center gap-5">
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :imageUrl="item.imgUrl"
      :price="item.price"
      :onClickFavorite="() => emit('addToFavorite', item)"
      :onClickAdd="() => emit('addToCart', item)"
      :isFavorite="item.isFavorite"
      :isAdded="item.isAdded"
    />
  </div>
</template>
