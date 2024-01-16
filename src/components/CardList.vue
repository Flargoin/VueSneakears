<script setup>
  import Card from './Card.vue';

    defineProps({
      items: Array
    })

    const onClickAdd = () => {
        alert('Доьавить!')
    }

    const emit = defineEmits(['addToFavorited', 'addToCart']);

</script>

<template>
    <div class="grid grid-cols-4 gap-5">
      <Card 
        v-for="item in items"
        :key="item.id"
        :id="item.id"
        :price="item.price" 
        :title="item.name" 
        :image-url="item.imageUrl"
        :onClickAdd="() => emit('addToCart', item)"
        :onClickFavorited ="() => emit('addToFavorited', item)"
        :isFavorited = "item.isFavorited"
        :isAdded = "item.isAdded"
      />
    </div>
</template>