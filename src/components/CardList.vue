<script setup>
import Card from './Card.vue'
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      title="Мужские кроссовки Nike Blazer Mid Suede"
      imageUrl="/sneakers/sneakers-1.jpg"
      :price="1200"
      :isAdded="false"
      :isFavorite="false"
    />
  </div>
</template>
