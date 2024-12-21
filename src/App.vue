<script setup>
import { onMounted, provide, reactive, ref, watch, computed } from 'vue'
import axios from 'axios'
import CardList from './components/CardList.vue'
import Header from './components/Header.vue'
import Drawer from './components/Drawer.vue'

const items = ref([])
const cart = ref([])
const isCreatingOrder = ref(false)
const drawerOpen = ref(false)
const totalPrice = computed(() =>
  cart.value.reduce((acc, item) => acc + item.price, 0)
)
const vatPrice = computed(() =>
  Math.round((totalPrice.value * 20) / 100)
)
const cartIsEmpty = computed(() =>
  cart.value.length === 0
)
const cartButtonDisabled = computed(() =>
  isCreatingOrder.value || cartIsEmpty.value
)
const openDrawer = () => {
  drawerOpen.value = true
}
const closeDrawer = () => {
  drawerOpen.value = false
}
const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
})
const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}
const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}
const createOrder = async () => {
  try {
    isCreatingOrder.value = true
    const { data } = await axios.post('https://8978a95badfad524.mokky.dev/orders', {
      items: cart.value,
      totalPrice: totalPrice.value,
    })
    cart.value = []
    return data
  } catch (error) {
    console.log(error)
  } finally {
    isCreatingOrder.value = false
  }
}
const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
  console.log(cart)
}
const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}
const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value
}
const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://8978a95badfad524.mokky.dev/favorites')
    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id)
      if (!favorite) {
        return item
      }
      return {
        ...item,
        isLiked: true,
        likedId: favorite.id,
      }
    })
  } catch (error) {
    console.log(error)
  }
}
const addToFavorite = async (item) => {
  try {
    if (!item.isLiked) {
      const obj = {
        parentId: item.id,
      }
      item.isLiked = true
      const { data } = await axios.post('https://8978a95badfad524.mokky.dev/favorites', obj)
      item.likedId = data.id
    } else {
      item.isLiked = false
      await axios.delete(`https://8978a95badfad524.mokky.dev/favorites/${item.likedId}`)
      item.favoriteId = null
    }
  } catch (error) {
    console.log(error)
  }
}
const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy,
    }
    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }
    const { data } = await axios.get('https://8978a95badfad524.mokky.dev/items?', {
      params,
    })
    items.value = data.map((item) => ({
      ...item,
      isAdded: false,
      likedId: null,
      isLiked: false,
    }))
  } catch (error) {
    console.log(error)
  }
}
onMounted(async () => {
  const localCart = localStorage.getItem('cart')
  cart.value = localCart ? JSON.parse(localCart) : []

  await fetchItems()
  await fetchFavorites()

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
  }))
})
watch(filters, fetchItems)
watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})
watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  {deep: true}
)
provide('cart', {
  openDrawer,
  closeDrawer,
  cart,
  addToCart,
  removeFromCart,
})
</script>
<template>
  <Drawer
    v-if="drawerOpen"
    :totalPrice="totalPrice"
    :vatPrice="vatPrice"
    @createOrder="createOrder"
    :buttonDisabled="cartButtonDisabled"
    />
  <div class="w-4/5 m-auto bg-white rounded-xl mt-20">
    <Header :totalPrice="totalPrice" @openDrawer="openDrawer" />
    <div class="p-5">
      <div class="flex justify-between items-center">
        <h2 class="text-2xl font-bold">Все кроссовки</h2>
        <select @change="onChangeSelect" class="p-2 border outline-none" name="" id="">
          <option value="name">по названию</option>
          <option value="price">по цене (сначала дешевле)</option>
          <option value="-price">по цене (сначала дороже)</option>
        </select>
        <div class="relative">
          <img class="absolute left-2 top-2.5" src="/search.svg" alt="search" />
          <input
            @input="onChangeSearchInput"
            placeholder="Поиск..."
            class="border pl-7 p-1.5 outline-none"
          />
        </div>
      </div>
      <CardList :items="items" @addToFavorite="addToFavorite" @addToCart="onClickAddPlus" />
    </div>
  </div>
</template>
<style scoped></style>
