<script setup>
import SneakersCard from './SneakersCard.vue'

defineProps({
  items: Array
})
const emit = defineEmits(['addToFavorite', 'addToCart']);
</script>
<template>
  <div class="grid grid-cols-4 gap-5 p-5">
    <SneakersCard
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :imageUrl="item.imageUrl"
      :title="item.title"
      :price="item.price"
      :isAdded="item.isAdded"
      :isLiked="item.isLiked"
      :onClickLike="() => emit('addToFavorite', item)"
      :onClickAdd="() => emit('addToCart', item)"
    />
  </div>
</template>
