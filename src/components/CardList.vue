<script setup>
import SneakersCard from './SneakersCard.vue'
defineProps({
  items: Array
})
const onClickAdd = () => {
  console.log('add')
}
</script>
<template>
  <div class="grid grid-cols-4 gap-5 p-5">
    <SneakersCard
      v-for="item in items"
      :key="item.id"
      :imageUrl="item.imageUrl"
      :title="item.title"
      :price="item.price"
      :isAdded="false"
      :isLiked="false"
      :onClickAdd="onClickAdd"
      :onClickLike="onClickAdd"
    />
  </div>
</template>
