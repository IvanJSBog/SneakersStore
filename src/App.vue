<script setup>
import CardList from './components/CardList.vue'
import Header from './components/Header.vue'
import Drawer from './components/Drawer.vue'
</script>

<template>
  <Drawer />
  <div class="w-4/5 m-auto bg-white rounded-xl mt-20">
    <Header />
    <div class="p-5">
      <h2 class="text-2xl font-bold">Все кроссовки</h2>
      <CardList />
    </div>
  </div>
</template>

<style scoped></style>
