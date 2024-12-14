<script setup>
import CartListItem from './CartListItem.vue'
</script>
<template>
  <div class="fixed top-0 left-0 w-full h-full bg-black z-10 opacity-50"></div>
  <div class="bg-white fixed top-0 right-0 w-1/3 h-full z-20 p-10">
    <div class="flex items-center gap-10 mb-6">
      <svg
        class="cursor-pointer opacity-50 hover:opacity-100 rotate-180"
        width="16"
        height="14"
        viewBox="0 0 16 14"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          d="M1 7H14.7143"
          stroke="black"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
        <path
          d="M8.71436 1L14.7144 7L8.71436 13"
          stroke="black"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
      </svg>

      <h2 class="text-2xl font-bold">Корзина</h2>
    </div>
    <CartListItem />

    <div class="flex flex-col gap-4">
      <div class="flex">
        <span>Итого:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>12900 $</b>
      </div>

      <div class="flex">
        <span>Налог: 5%</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>900 $</b>
      </div>
      <button
        disabled=""
        class="bg-blue-700 text-white p-3 rounded-xl active:bg-blue-800 cursor-pointer disabled:opacity-50"
      >
        <span>Оформить заказ</span>
      </button>
    </div>
  </div>
</template>
