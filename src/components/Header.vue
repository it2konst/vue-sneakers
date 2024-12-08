<script setup>
defineProps({
    totalPrice: Number,
})

const emit = defineEmits(['toggleDrawer'])
</script>

<template>
    <header
        class="flex flex-wrap gap-4 justify-between border-b border-slate-200 px-10 py-4 md:px-5 md:py-8"
    >
        <div class="flex items-center gap-3">
            <img src="/logo.png" alt="Logo" class="w-10" />
            <div>
                <h2 class="text-xl font-bold uppercase text-lg sm:text-xl">Vue Sneakers</h2>
                <p class="text-slate-400 text-sm sm:text-base">Магазин лучших кроссовок</p>
            </div>
        </div>
        <ul class="flex flex-wrap items-center gap-4 sm:gap-10 text-sm sm:text-base">
            <li
                @click="emit('toggleDrawer')"
                class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black"
            >
                <img src="/cart.svg" alt="Cart" />
                <b>{{ totalPrice }} руб.</b>
            </li>
            <li
                class="flex items-center cursor-pointer gap-3 text-sm sm:text-base text-gray-500 hover:text-black"
            >
                <img src="/heart.svg" alt="Heart" />
                <span>Закладки</span>
            </li>
            <li
                class="flex items-center cursor-pointer gap-3 text-sm sm:text-base text-gray-500 hover:text-black"
            >
                <img src="/profile.svg" alt="Profile" />
                <span>Профиль</span>
            </li>
        </ul>
    </header>
</template>
