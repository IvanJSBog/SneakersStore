<script setup>
const emit = defineEmits(['openDrawer'])
defineProps({
  totalPrice: Number,
})
</script>
<template>
  <header class="flex justify-between items-center p-5 border-b bg-white rounded-xl">
    <div class="flex items-center gap-5">
      <img src="/logo.png" alt="logo" class="w-16" />
      <div>
        <h2 class="font-bold uppercase">Sneakers Store</h2>
        <p>Магазин кроссовок</p>
      </div>
    </div>
    <ul class="flex items-center gap-10">
      <li
        @click="() => emit('openDrawer')"
        class="flex items-center gap-5 hover:text-black text-gray-500 cursor-pointer"
      >
        <img src="/cart.svg" alt="cart" />
        <p>{{ totalPrice }} $</p>
      </li>
      <li class="flex items-center gap-5 hover:text-black text-gray-500 cursor-pointer">
        <img src="/heart.svg" alt="heart" />
        <p>Закладки</p>
      </li>
      <li class="flex items-center gap-5 hover:text-black text-gray-500 cursor-pointer">
        <img src="/profile.svg" alt="profile" />
        <p>Профиль</p>
      </li>
    </ul>
  </header>
</template>
