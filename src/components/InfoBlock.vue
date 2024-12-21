<script setup>
  defineProps({
    imageUrl: String,
    title: String,
    description: String
  })
</script>
<template>
  <div class="flex flex-col items-center">
    <img width="70" height="70" :src="imageUrl" alt="pusto">
    <h2 class="mt-4 text-2xl font-medium">{{ title }}</h2>
    <p>{{ description }}</p>
  </div>
</template>
