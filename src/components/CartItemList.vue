<script setup>
import { inject } from 'vue'
import CartItem from './CartItem.vue'

const { cart } = inject('cart')
</script>

<template>
  <div class="flex flex-col gap-2">
    <CartItem
      v-for="item in cart"
      :key="item.id"
      :title="item.title"
      :imageUrl="item.imgUrl"
      :price="item.price"
    />
  </div>
</template>
