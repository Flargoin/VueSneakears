<script setup>
    defineProps({
        imageUrl: String,
        title: String,
        price: Number,
        isFavorited: Boolean,
        isAdded: Boolean,
        onClickAdd: Function,
        onClickFavorite: Function
    })
</script>

<template>
  <div>
    <div
      class="relative bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer transition hover:-translate-y-2 hover:shadow-xl"
    >
      <img 
        class="absolute top-8 left-8" 
        :src="!isFavorited ? '/public/like-1.svg' : '/public/like-2.svg'" 
        alt="Like state" 
        @click="onClickFavorite"
      />

      <img :src="imageUrl" alt="sneaker" />
      <p class="mt-2">{{ title }}</p>

      <div class="flex justify-between mt-5">
        <div class="flex flex-col">
          <span class="text-slate=400">Цена:</span>
          <b>{{ price }} руб.</b>
        </div>

        <img 
            :src="!isAdded ? '/public/plus.svg' : '/public/checked.svg'" 
            alt="added state" 
            @click="onClickAdd"
        />
      </div>
    </div>
  </div>
</template>
