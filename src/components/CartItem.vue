<script setup>
const emit = defineEmits(['onClickRemove'])
defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number,
})
</script>
<template>
  <div class="flex border p-5 items-center gap-10">
    <img class="w-16 h-16" :src="imageUrl" :alt="title" />
    <div class="flex flex-col flex-1">
      <p>{{ title }}</p>
      <div class="flex justify-between">
        <b>{{ price }} $</b>
        <img
          @click="emit('onClickRemove')"
          src="/close.svg"
          alt="close"
          class="cursor-pointer opacity-50 hover:opacity-100"
        />
      </div>
    </div>
  </div>
</template>
