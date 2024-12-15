<script setup>
defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number,
  isLiked: Boolean,
  isAdded: Boolean,
  onClickLike: Function,
  onClickAdd: Function
})

</script>
<template>
  <div
    class="relative bg-white p-8 border cursor-pointer rounded-xl hover:translate-y-2 hover:shadow-2xl"
  >
    <img
      @click="onClickLike"
      :src="isLiked ? '/like-2.svg' : '/like-1.svg'"
      alt="like"
      class="absolute top-8 left-8"
    />
    <img :src="imageUrl" alt="Sneakers" />
    <p>{{ title }}</p>

    <div class="flex justify-between">
      <div class="flex flex-col">
        <span class="font-bold">Цена:</span>
        <span>{{ price }} $</span>
      </div>

      <img @click="onClickAdd" :src="isAdded ? '/checked.svg' : '/plus.svg'" alt="Plus" />
    </div>
  </div>
</template>
