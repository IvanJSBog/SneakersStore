<script setup>
import SneakersCard from './SneakersCard.vue'
const onClickAdd = () => {
  console.log('add')
}
</script>
<template>
  <div class="grid grid-cols-4 gap-5 p-5">
    <SneakersCard
      imageUrl="/sneakers/sneakers-1.jpg"
      title="Nike Air Max 90"
      :price="1205"
      :isAdded="false"
      :isLiked="false"
      :onClickAdd="onClickAdd"
      :onClickLike="onClickAdd"
    />
  </div>
</template>
