<script setup>
import { reactive, watch, ref, onMounted } from 'vue'
import axios from 'axios'
import debounce from 'lodash.debounce'
import { inject } from 'vue'
import CardList from './../components/CardList.vue'

const { cart, addToCart, removeFromCart } = inject('cart')

const items = ref([])

const filters = reactive({
    sortBy: 'title',
    searchQuery: '',
})

const onClickAddPlus = (item) => {
    if (!item.isAdded) {
        addToCart(item)
    } else {
        removeFromCart(item)
    }
}

const onChangeSelect = (event) => {
    filters.sortBy = event.target.value
}

const onChangeSearchInput = (event) => {
    filters.searchQuery = event.target.value
}

const debounceSearchInput = debounce(onChangeSearchInput, 500)

const addToFavorite = async (item) => {
    // item.isFavorite = !item.isFavorite
    try {
        if (!item.isFavorite) {
            const obj = {
                // parentId: item.id,
                // item,
                item_id: item.id,
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

const fetchFavorites = async () => {
    try {
        const { data: favorites } = await axios.get('https://8b444cda99b13d6c.mokky.dev/favorites')

        items.value = items.value.map((item) => {
            const favItem = favorites.find((favItem) => favItem.item_id === item.id)
            // const favItem = favorites.find((favItem) => favItem.parentId === item.id)

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

const fetchItems = async () => {
    try {
        const params = {
            sortBy: filters.sortBy,
        }
        if (filters.searchQuery) {
            params.title = `*${filters.searchQuery}*`
        }
        const { data } = await axios.get('https://8b444cda99b13d6c.mokky.dev/items', { params })
        // const { data } = await axios.get('./data.json', { params })
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
    const localStorageCart = localStorage.getItem('sneakers-cart')
    cart.value = localStorageCart ? JSON.parse(localStorageCart) : []

    await fetchItems()
    await fetchFavorites()

    items.value = items.value.map((item) => ({
        ...item,
        isAdded: cart.value.some((cartItem) => cartItem.id === item.id),
    }))
})

watch(cart, () => {
    items.value = items.value.map((item) => ({
        ...item,
        isAdded: false,
    }))
})

watch(filters, fetchItems)

console.log('Favorites component loaded')
</script>

<template>
    <div class="flex flex-wrap justify-center lg:justify-between items-center">
        <h2 class="text-3xl font-bold text-xl sm:text-2xl mb-4 mr-4 md:mb-8">Все кроссовки</h2>

        <div class="flex flex-wrap gap-4 mb-8 justify-center lg:justify-between">
            <select @change="onChangeSelect" class="py-2 px-4 border rounded-md outline-none">
                <option value="name">По названию</option>
                <option value="price">По цене (по возрастанию)</option>
                <option value="-price">По цене (по убыванию)</option>
            </select>

            <div class="relative">
                <img class="absolute left-4 top-3" src="/search.svg" alt="Search" />
                <input
                    @input="debounceSearchInput"
                    class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
                    type="text"
                    placeholder="Поиск..."
                />
            </div>
        </div>
    </div>
    <CardList :items="items" @addToFavorite="addToFavorite" @addToCart="onClickAddPlus" />
</template>

<style scoped>
select,
input {
    max-width: 210px;
    width: 100%;
}
</style>
