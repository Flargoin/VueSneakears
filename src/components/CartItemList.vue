<script setup>
    import { inject } from 'vue';
    import Cartitem from './CartItem.vue'

    const { cart, removeFromCart } = inject('cart');
</script>

<template>
    <div 
    class="flex flex-col gap-5">
        <Cartitem v-for="item in cart" 
        :item="item.id" 
        :title="item.name"
        :price="item.price"
        :image-url="item.imageUrl"
        @on-click-remove="() => removeFromCart(item)"
        />   
    </div>
</template>