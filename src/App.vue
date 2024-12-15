<script setup>
import { onMounted, reactive, ref, watch } from 'vue'
import axios from 'axios'

import CardList from './components/CardList.vue'
import Header from './components/Header.vue'
import Drawer from './components/Drawer.vue'

const items = ref([])
const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
})

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

    // items.value = data
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
  await fetchItems()
  await fetchFavorites()
})
watch(filters, fetchItems)
</script>

<template>
  <!-- <Drawer /> -->
  <div class="w-4/5 m-auto bg-white rounded-xl mt-20">
    <Header />

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
      <CardList :items="items" @addToFavorite="addToFavorite" />
    </div>
  </div>
</template>

<style scoped></style>
