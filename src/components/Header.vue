<script setup> 
  defineProps({
    totalPrice: Number
  })

  const emit = defineEmits(['openDrawer']);
</script>

<template>
    <header class="flex justify-between sticky top-0 z-10 bg-white border-b border-slate-200 px-10 py-8">
      <div class="flex items-center gap-4">
        <img class="w-10" src="/public/logo.png" alt="Logo" />

        <div>
          <h2 class="text-xl font-bold uppercase">Vue Sneakears</h2>
          <p class="text-slate-400">Магазин лучших кроссовок</p>
        </div>
      </div>

      <ul class="flex items-center gap-10">
        <li class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black"
        @click="() => emit('openDrawer')">
          <img src="/public/cart.svg" alt="">
          <b>{{totalPrice}} руб.</b>
        </li>

        <li class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black">
          <img src="/public/heart.svg" alt="">
          <span>Закладки</span>
        </li>

        <li class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black">
          <img src="/public/profile.svg" alt="">
          <span>Профиль</span>
        </li>
      </ul>
    </header>
</template>