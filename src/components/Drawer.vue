<script setup>
import { onMounted, onBeforeUnmount } from 'vue'

import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'

defineProps({
    totalPrice: Number,
    vatPrice: Number,
})

const emit = defineEmits(['toggleDrawer'])

onMounted(() => {
    document.documentElement.style.overflowY = 'hidden'
})

onBeforeUnmount(() => {
    document.documentElement.style.overflowY = ''
})
</script>

<template>
    <div
        class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"
        @click="emit('toggleDrawer')"
    ></div>
    <div class="fixed top-0 right-0 h-full w-96 bg-gray-300 z-20 p-8">
        <DrawerHead />

        <CartItemList />

        <div class="flex flex-col gap-4 mt-7">
            <div class="flex gap-2">
                <span>Итого:</span>
                <div class="flex-1 border-b border-dashed"></div>
                <b>{{ totalPrice }} ₽</b>
            </div>

            <div class="flex gap-2">
                <span>Налог 5%</span>
                <div class="flex-1 border-b border-dashed"></div>
                <b>{{ vatPrice }} ₽</b>
            </div>

            <button
                class="mt-4 bg-lime-500 w-full rounded-xl p-3 text-white hover:bg-lime-600 active:bg-lime-700 transition-all duration-300 cursor-pointer disabled:bg-slate-400"
            >
                Оформить заказ
            </button>
        </div>
    </div>
</template>
