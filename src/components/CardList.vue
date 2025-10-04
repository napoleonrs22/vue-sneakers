<script setup>
import Card from './Card.vue';


defineProps({
    items: Array
})

const onclickAdd  = () =>{
    alert('add')
}
</script>

<template>
    <div class="grid grid-cols-4 gap-5 ">
        <Card
            v-for="item in items"
            :key="item.id"
            :title="item.title"
            :price="item.price"
            :imageUrl="item.imageUrl"
            :is-added="false"
            :is-favorite="true"
            :onclickAdd="onclickAdd"
        />
    </div>
</template>