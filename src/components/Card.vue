<script setup>
defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickFavorite: Function,
  onClickAdd: Function,
})
</script>

<template>
  <div
    class="product-card relative bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer hover:-translate-y-2 hover:shadow-2xl transition-all duration-300 text-sm sm:text-base"
  >
    <img
      :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
      alt="Like 1"
      class="absolute top-8 left-8"
      @click="onClickFavorite"
    />

    <img class="card-image" :src="imageUrl" alt="Sneaker" />
    <p class="mt-2">{{ title }}</p>

    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-200">Цена:</span>
        <b>{{ price }} руб.</b>
      </div>

      <img :src="!isAdded ? '/plus.svg' : '/checked.svg'" alt="Plus" />
    </div>
  </div>
</template>

<style scoped>
.product-card {
  max-width: 210px;
  width: 100%;
}

.card-image {
  display: block;
  height: 120px;
  width: 100%;
  object-fit: cover;
  margin: 0 auto;
  border-radius: 8px;
}
</style>
