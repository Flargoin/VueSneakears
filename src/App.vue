
<script setup>
  import { computed, onMounted, provide, reactive, ref, watch } from 'vue';
  import axios from 'axios';

  import Drawer from './components/Drawer.vue';
  import Header from './components/Header.vue';
  import CardList from './components/CardList.vue';

  const items = ref([]);
  const cart = ref([]);
  const isCreatingOrder = ref(false);

  const drawerOpen = ref(false);

  const closeDrawer = () => {
    drawerOpen.value = false;
  }

  const openDrawer = () => {
    drawerOpen.value = true;
  }

  const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0));
  const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100));

  const cartIsEmpty = computed(() => cart.value.length === 0);

  const cartButtonDisabled = computed(() => isCreatingOrder.value || cartIsEmpty.value);

  const filters = reactive({
    sortBy: 'name',
    searchQuary: ''
  });

  const addToCart = (item) => {
      cart.value.push(item);
      item.isAdded = true;
  }

  const removeFromCart = (item) => {
      cart.value.splice(cart.value.indexOf(item), 1);
      item.isAdded = false;
  }

  const createOrder = async () => {
    try{
      isCreatingOrder.value = true;
      const { data } = await axios.post(`https://65ae38eeee4ab932.mokky.dev/orders`, {
        items: cart.value,
        totalPrice: totalPrice.value,
      })
      cart.value = [];

      return data;
    } catch(err) {
      console.log(err);
    } finally {
      isCreatingOrder.value = false;
    }
  }

  const onClickAddPlus = (item) => {
    if(!item.isAdded) {
      addToCart(item);
    } else {
      removeFromCart(item);
    }  

    console.log(cart.value);
  }

  const onChangeSelect = (event) => {
    filters.sortBy = event.target.value;
  }

  const onChangeSearchInput = (event) => {
    filters.searchQuary = event.target.value;
  }

  const fetchFavorites = async () => {
    try {
        const { data: favorites } = await axios.get(`https://65ae38eeee4ab932.mokky.dev/favorites`);
        
        items.value = items.value.map((item) => {
          const favorite = favorites.find(favorite => favorite.parentId === item.id);

          if(!favorite) {
            return item;
          }

          return {
            ...item,
            isFavorited: true,
            favoriteId: favorite.id
          }
        });

        console.log(items.value);
      } catch(err) {
        console.log(err);
      }
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
          favoriteId: null,
          isAdded: false
        }));
      } catch(err) {
        console.log(err);
      }
  }

  const addToFavorited = async (item) => {
    try {
      if(!item.isFavorited) {
        const obj = {
        parentId: item.id,
      }

      item.isFavorited = !item.isFavorited;
      const { data } = await axios.post(`https://65ae38eeee4ab932.mokky.dev/favorites`, obj);
      item.favoriteId = data.id;


      console.log(data);
    } else {
      item.isFavorited = false;
      const { data } = await axios.delete(`https://65ae38eeee4ab932.mokky.dev/favorites/${item.favoriteId}`);
      item.favoriteId = null;
    }
    } catch(e) {
      console.log(e);
    }
  }

  onMounted(async () => {
    await fetchItems();
    await fetchFavorites();
  });
  watch(filters, fetchItems);

/*   watch(cart, () => {
    items.value = items.value.map(item => ({
      ...item,
      isAdded: false
    })
  }); */

  provide("cart", {
    cart,
    closeDrawer,
    openDrawer,
    addToCart,
    removeFromCart
  });
</script>

<template>
  <Drawer 
  v-if="drawerOpen" 
  :total-price="totalPrice" 
  :vat-price="vatPrice" 
  :button-disabled = "cartButtonDisabled"
  @create-order="createOrder"/>

 <div
  class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14"
 >
  <Header :total-price="totalPrice" @open-drawer="openDrawer" />


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
      <CardList :items="items" @add-to-favorited="addToFavorited" @add-to-cart="onClickAddPlus"/>
    </div>
  </div>

 </div>

</template>

<style scoped>

</style>
