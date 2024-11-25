<script setup>
import Drawer from './components/Drawer.vue'
import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
</script>

<template>
  <!-- <Drawer /> -->
  <div class="w-4/5 m-auto rounded-xl shadow-xl mt-14 bg-gray-400">
    <Header />
    <div class="p-10">
      <h2 class="text-3xl font-bold m-8">Все кроссовки</h2>
      <CardList />
    </div>
  </div>
</template>

<style></style>
