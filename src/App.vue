<script setup>
import { ref, provide, watch, computed } from 'vue'

import Header from './components/Header.vue'
import Drawer from './components/Drawer.vue'

/* Корзина (Start)*/
const cart = ref([])
const drawerOpen = ref(false)

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))

const toggleDrawer = () => {
    drawerOpen.value = !drawerOpen.value
}

const addToCart = (item) => {
    item.isAdded = true
    cart.value.push(item)
}

const removeFromCart = (item) => {
    item.isAdded = false
    // cart.value.splice(cart.value.indexOf(item), 1)
    cart.value = cart.value.filter((i) => i.id !== item.id)
}

watch(
    cart,
    () => {
        localStorage.setItem('sneakers-cart', JSON.stringify(cart.value))
    },
    { deep: true },
)

provide('cart', { cart, toggleDrawer, addToCart, removeFromCart })
/* Корзина (End)*/
</script>

<template>
    <Drawer
        v-if="drawerOpen"
        @toggleDrawer="toggleDrawer"
        :totalPrice="totalPrice"
        :vatPrice="vatPrice"
    />

    <div class="container m-auto rounded-xl shadow-xl mt-14 bg-slate-50">
        <Header :totalPrice="totalPrice" @toggleDrawer="toggleDrawer" />

        <div class="p-4 md:p-10">
            <!-- <router-view v-slot="{ Component }">
                <transition name="fade" mode="out-in">
                    <component :is="Component" />
                </transition>
            </router-view> -->
            <router-view></router-view>
        </div>
    </div>
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.5s;
}
.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}
</style>
