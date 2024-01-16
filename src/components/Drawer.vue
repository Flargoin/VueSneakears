<script setup>
import { computed } from 'vue';
    import DrawerHead from './DrawerHead.vue';
    import CartItemList from './CartItemList.vue';

    const emit = defineEmits(['createOrder']);

    defineProps({
        totalPrice: Number,
        vatPrice: Number,
        buttonDisabled: Boolean
    })

</script>

<template>
    <div class="fixed top-0 left-0 h-full w-full bg-black z-20 opacity-70"></div>

    <div class="bg-white w-96 h-full fixed flex flex-col right-0 top-0 z-20 p-8">
        <DrawerHead />


        <CartItemList />

        <div class="flex flex-col gap-4 mt-auto">
            <div class="flex gap-2">
                <span>Итого:</span>
                <div class="flex-1 border-b border-dashed"></div>
                <b>{{ totalPrice }} руб.</b>
            </div>

            <div class="flex gap-2">
                <span>Налог 5%:</span>
                <div class="flex-1 border-b border-dashed"></div>
                <b>{{ vatPrice }} руб. </b>
            </div>

        <button 
            :disabled = "buttonDisabled"
            class="mt-4 bg-lime-500 w-full rounded-xl py-3 text-white transition disabled:bg-slate-300 hover:bg-lime-600 active:bg-lime-700 cursor-pointer"
            @click="() => emit('createOrder')">
            {{ isCreatingOrder ?  "Заказ оформлен" : "Оформить заказ"}}
        </button>
        </div>
    </div>
</template>