<script setup>
import Card from './Card.vue';


const onclickAdd  = () =>{
    alert('addd')
}
</script>

<template>
    <div class="grid grid-cols-4 gap-5 ">
        <Card 
            title="Мужские Кроссовки Nike Blazer Mid Suede"
            :price="1205"
            imageUrl="/sneakers/sneakers-1.jpg"
            :is-added="false"
            :is-favorite="true"
            :onclickAdd="onclickAdd"
        />
        <Card/>
        <Card/>
        <Card/>
        <Card/>
        <Card/>
    </div>
</template>