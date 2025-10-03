<script setup>

import DrawedHead from './DrawedHead.vue';
import CardItemList from './CardItemList.vue';
</script>
<template>
    <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
    <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
        <DrawedHead/>

        <CardItemList/>

        <div class="flex-shrink-0">
            <div class="flex flex-col gap-4 mb-4 my-7">
                <div class="flex gap-2">
                    <span>Итого:</span>
                    <div class="flex-1 border-b border-dashed border-gray-300"></div>
                    <b>12 990 ₽</b>
                </div>

                <div class="flex gap-2">
                    <span>Налог 5%</span>
                    <div class="flex-1 border-b border-dashed border-gray-300"></div>
                    <b>999 ₽</b>
                </div>

                <button class=" mt-4 bg-lime-500 hover:bg-lime-600 w-full rounded-xl py-3 text-white font-semibold transition-colors">
                    Оформить заказ
                </button>
            </div>
        </div>
    </div>
</template>