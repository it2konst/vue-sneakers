<script setup>
import { onMounted, reactive, ref, watch, provide, computed } from 'vue'
import axios from 'axios'

import Drawer from './components/Drawer.vue'
import Header from './components/Header.vue'
import CardList from './components/CardList.vue'

const items = ref([])
const cart = ref([])

const drawerOpen = ref(false)

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))

const toggleDrawer = () => {
    drawerOpen.value = !drawerOpen.value
}

const filters = reactive({
    sortBy: 'title',
    searchQuery: '',
})

const addToCart = (item) => {
    item.isAdded = true
    cart.value.push(item)
}

const removeFromCart = (item) => {
    item.isAdded = false
    // cart.value.splice(cart.value.indexOf(item), 1)
    cart.value = cart.value.filter((i) => i.id !== item.id)
}

const onClickAddPlus = (item) => {
    if (!item.isAdded) {
        addToCart(item)
    } else {
        removeFromCart(item)
    }
    // console.log(cart.value)
}

const onChangeSelect = (event) => {
    filters.sortBy = event.target.value
}

const onChangeSearchInput = (event) => {
    filters.searchQuery = event.target.value
}

const fetchFavorites = async () => {
    try {
        const { data: favorites } = await axios.get('https://8b444cda99b13d6c.mokky.dev/favorites')

        items.value = items.value.map((item) => {
            const favItem = favorites.find((favItem) => favItem.parentId === item.id)
            // return {...item, isFavorite: !!favoriteItem}
            if (!favItem) {
                return item
            }

            return { ...item, isFavorite: true, favItemId: favItem.id }
        })
        // console.log(items.value)
    } catch (err) {
        console.log(err)
    }
}

const addToFavorite = async (item) => {
    // item.isFavorite = !item.isFavorite
    try {
        if (!item.isFavorite) {
            const obj = {
                parentId: item.id,
            }
            item.isFavorite = true
            const { data } = await axios.post('https://8b444cda99b13d6c.mokky.dev/favorites', obj)
            item.favItemId = data.id
        } else {
            item.isFavorite = false
            await axios.delete(`https://8b444cda99b13d6c.mokky.dev/favorites/${item.favItemId}`)
            item.favItemId = null
        }
    } catch (err) {
        console.log(err)
    }
}

const fetchItems = async () => {
    try {
        const params = {
            sortBy: filters.sortBy,
        }
        if (filters.searchQuery) {
            params.title = `*${filters.searchQuery}*`
        }
        const { data } = await axios.get('https://8b444cda99b13d6c.mokky.dev/items', { params })
        items.value = data.map((obj) => ({
            ...obj,
            isFavorite: false,
            favItemId: null,
            isAdded: false,
        }))
    } catch (err) {
        console.log(err)
    }
}

onMounted(async () => {
    await fetchItems()
    await fetchFavorites()
})

watch(filters, fetchItems)

provide('cart', { cart, toggleDrawer, addToCart, removeFromCart })
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
            <div class="flex flex-wrap justify-center lg:justify-between items-center">
                <h2 class="text-3xl font-bold text-xl sm:text-2xl mb-4 mr-4 md:mb-8">
                    Все кроссовки
                </h2>

                <div class="flex flex-wrap gap-4 mb-8 justify-center lg:justify-between">
                    <select
                        @change="onChangeSelect"
                        class="py-2 px-4 border rounded-md outline-none"
                    >
                        <option value="name">По названию</option>
                        <option value="price">По цене (по возрастанию)</option>
                        <option value="-price">По цене (по убыванию)</option>
                    </select>

                    <div class="relative">
                        <img class="absolute left-4 top-3" src="/search.svg" alt="Search" />
                        <input
                            @input="onChangeSearchInput"
                            class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
                            type="text"
                            placeholder="Поиск..."
                        />
                    </div>
                </div>
            </div>
            <CardList :items="items" @addToFavorite="addToFavorite" @addToCart="onClickAddPlus" />
        </div>
    </div>
</template>

<style scoped>
select,
input {
    max-width: 210px;
    width: 100%;
}
/*
<div class="flex flex-wrap justify-center gap-4 md:justify-start lg:justify-center">
sm:	640px
md:	768px
lg:	1024px
xl:	1280px
*/

/* .drawer-enter-active,
.drawer-leave-active {
  transition: all 0.5s ease;
}

.drawer-enter-from,
.drawer-leave-to {
  opacity: 0;
  transform: translateX(-100%);
}

.drawer-enter-to,
.drawer-leave-from {
  opacity: 1;
  transform: translateX(0);
} */
</style>
