<script setup>
    import Card from './Card.vue';

    defineProps({
      items: Array
    })

    const onClickAdd = () => {
        alert('Доьавить!')
    }
</script>

<template>
    <div class="grid grid-cols-4 gap-5">
      <Card 
        v-for="item in items"
        :key="item.id"
        :price="item.price" 
        :title="item.name" 
        :image-url="item.imageUrl"
        :onClickAdd="onClickAdd"
        :isFavorited = "item.isFavorited"
      />
    </div>
</template>