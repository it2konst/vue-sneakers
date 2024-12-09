<script setup>
import axios from 'axios'
import { onMounted, onBeforeUnmount, ref, computed, inject } from 'vue'

import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'
import InfoBlock from './InfoBlock.vue'

const emit = defineEmits(['toggleDrawer'])

const props = defineProps({
    totalPrice: Number,
    vatPrice: Number,
})

const { cart } = inject('cart')

const isCreating = ref(false)
const orderId = ref(null)

const createOrder = async () => {
    isCreating.value = true
    try {
        const { data } = await axios.post('https://8b444cda99b13d6c.mokky.dev/orders', {
            items: cart.value,
            totalPrice: props.totalPrice.value,
        })
        cart.value = []
        orderId.value = data.id

        return data
    } catch (err) {
        console.log(err)
    } finally {
        isCreating.value = false
    }
}

const cartIsEmpty = computed(() => cart.value.length === 0)

const buttonDisabled = computed(() => (cartIsEmpty.value || isCreating.value ? true : false))

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
        @click="toggleDrawer"
    ></div>
    <div class="fixed top-0 right-0 h-full w-96 bg-gray-300 z-20 p-8">
        <DrawerHead />

        <div v-if="!totalPrice || orderId" class="flex items-center h-full">
            <info-block
                v-if="!totalPrice && !orderId"
                title="Корзина пустая"
                description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
                image-url="./package-icon.png"
            />
            <InfoBlock
                v-if="orderId"
                title="Заказ оформлен!"
                :description="`Ваш заказ #${orderId} скоро будет передан курьерской доставке`"
                image-url="./order-success-icon.png"
            />
        </div>

        <div v-else="totalPrice" class="">
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
                    :disabled="buttonDisabled"
                    @click="createOrder"
                    class="mt-4 bg-lime-500 w-full rounded-xl p-3 text-white hover:bg-lime-600 active:bg-lime-700 transition-all duration-300 cursor-pointer disabled:bg-slate-400 shadow"
                >
                    Оформить заказ
                </button>
            </div>
        </div>
    </div>
</template>
