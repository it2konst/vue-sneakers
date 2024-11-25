<script setup>
import Card from './Card.vue'

defineProps({
  items: Array,
})

const onClickAdd = () => {
  alert('Add')
}
</script>

<template>
  <!-- <div class="grid grid-cols-4 gap-5"> -->
  <div class="flex flex-wrap gap-5">
    <Card
      v-for="item in items"
      :key="item.id"
      :title="item.title"
      :imageUrl="item.imgUrl"
      :price="item.price"
      :isFavorite="false"
      :onClickAdd="onClickAdd"
      :isAdded="false"
    />
  </div>
</template>
