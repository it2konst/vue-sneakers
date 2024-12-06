<script setup>
const emit = defineEmits(['onClickRemove'])

defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
})
</script>

<template>
  <div class="flex items-center gap-4 border border-slate-200 p-4 rounded-xl">
    <img class="w-16 h-16" :src="imageUrl" :alt="title" />

    <div class="flex flex-col justify-between w-full">
      <p>{{ title }}</p>

      <div class="flex items-center justify-between mt-2">
        <!-- <span class="text-slate-200">Цена:</span> -->
        <b>{{ price }}</b>
        <img
          @click="emit('onClickRemove')"
          class="cursor-pointer opacity-40 hover:opacity-100 transition-all duration-300"
          src="/close.svg"
          alt=""
        />
      </div>
    </div>
  </div>
</template>
