<script setup>
  import { onMounted, reactive, ref, watch } from 'vue';
  import axios from 'axios';

  import Drawer from './components/Drawer.vue';
  import Header from './components/Header.vue';
  import CardList from './components/CardList.vue';

  const items = ref([]);

  const filters = reactive({
    sortBy: 'name',
    searchQuary: ''
  });

  const onChangeSelect = (event) => {
    filters.sortBy = event.target.value;
  }

  const onChangeSearchInput = (event) => {
    filters.searchQuary = event.target.value;
  }

  const fetchFavorites = async () => {
    try {
        const { data: favorites } = await axios.get(`https://65ae38eeee4ab932.mokky.dev/favorites`);

        items.value = items.value.map(item => {
          const favorite = favorites.find(favorite => favorite.parentId === item.id);

          if(!favorite) {
            return item;
          }

          return {
            ...item,
            isFavorited: true,
            favoriteId: favorite.id
          }
        })
        
        
        console.log(data)
        items.value = data;
      } catch(err) {
        console.log(err);
      }
  }

  const addToFavorited = async (item) => {
    item.isFavorited = true;
  }

  const fetchItems = async () => {
      try {
        const params = {
          sortBy : filters.sortBy
        }

        if(filters.searchQuary) {
          params.name = `*${filters.searchQuary}*`;
        }

        const { data } = await axios.get(
          `https://65ae38eeee4ab932.mokky.dev/items`,
          {params}
        );
        
        items.value = data.map(obj => ({
          ...obj,
          isFavorited: false,
          isAdded: false
        }));
      } catch(err) {
        console.log(err);
      }
  }

  onMounted(async () => {
    await fetchItems();
    await fetchFavorites();
  });
  watch(filters, fetchItems);
</script>

<template>
  <!-- <Drawer /> -->
 <div
  class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14"
 >
  <Header />


  <div class="p-10">
    <div class="flex justify-between items-center">
      <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>

      <div class="flex gap-4">
        <select
          @change="onChangeSelect"
          class="bg-white py-2 px-3 border rounded-md outline-none focus:border-gray-400"
        >
          <option value="name">По названию</option>
          <option value="price">По цене (Дешевые)</option>
          <option value="-price">По цене (Дорогие)</option>
        </select>


        <div class="relative">
          <img class="absolute top-3 left-4" src="/public/search.svg" alt="">
          <input
            @input="onChangeSearchInput" 
            class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
            placeholder="Поиск"  
            type="text"
          >
        </div>
      </div>
    </div>

    <div class="mt-10">
      <CardList :items="items"/>
    </div>
  </div>

 </div>

</template>

<style scoped>

</style>
