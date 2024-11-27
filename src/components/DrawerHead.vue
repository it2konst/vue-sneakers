<script setup>
import { inject } from 'vue'

const { toggleDrawer } = inject('cart')
</script>

<template>
  <div class="flex items-center gap-5 mb-4">
    <svg
      @click="toggleDrawer"
      class="opacity-50 cursor-pointer rotate-180 hover:opacity-100 transition-all duration-300 hover:-translate-x-1"
      xmlns="http://www.w3.org/2000/svg"
      width="16"
      height="14"
      fill="none"
    >
      <path
        stroke="#000"
        stroke-linecap="round"
        stroke-linejoin="round"
        stroke-width="2"
        d="M1 7h13.714M8.714 1l6 6-6 6"
      />
    </svg>
    <h2 class="text-2xl font-bold">Корзина</h2>
  </div>
</template>
