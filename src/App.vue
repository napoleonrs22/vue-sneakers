<script setup>
import Header from './components/Header.vue';
import Drawer from './components/Drawer.vue';
import axios from 'axios';
import { computed, provide, ref, watch } from 'vue';
import Home from './pages/Home.vue';


const cart = ref([]);

const isCreatingOrder = ref(false);

const drawerOpen = ref(false);

// const totalPrice = cart.value.reduce((acc,item) => acc + item.price, 0)

const totalPrice = computed(
  () => cart.value.reduce((acc,item) => acc + item.price, 0)
);

const vatPrice = computed(
  () => Math.round((totalPrice.value * 5) /100 )
);

const cartIsEmpty = computed(() => cart.value.length === 0);

const cartButtonDisabled = computed (()  =>
 isCreatingOrder.value  || cartIsEmpty.value
);



const closeDrawer = () => {
  drawerOpen.value = false;
}
const openDrawer =  () => {
  drawerOpen.value = true;
}


const addToCart = (item) => {
  cart.value.push(item);
  item.isAdded = true
}


const removeFromCart = (item) => {
  const index = cart.value.findIndex(cartItem => cartItem.id === item.id);
  
  if (index !== -1) {
    cart.value.splice(index, 1);
  }

  item.isAdded = false; 
}

const createOrder = async () => {
  try{
    isCreatingOrder.value = true;
    const {data} =  await axios.post(`https://007c7bb5747041ef.mokky.dev/orders`,{
      items:cart.value,
      totalPrice: totalPrice.value,

    })

    cart.value = [];


    return data;
    // items:cart
    // totalPrice: totalPrice.value,
  }catch(err) {
    console.log(err);

  }finally {
    isCreatingOrder.value = false;
  }
}

const removeById = (id) => {
  const index = cart.value.findIndex(item => item.id === id);
  if (index !== -1) {
    cart.value.splice(index, 1);
  }
  
  const item = items.value.find(item => item.id === id);
  if (item) {
    item.isAdded = false;
  }
  
  console.log('Удалено из корзины:', id);
  console.log('Корзина после удаления:', cart.value);
}



watch(cart, ()=> {
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  {deep:true}
})

provide('cart',{
  cart,
  closeDrawer,
  openDrawer,
  addToCart,
  removeFromCart,
  removeById,
  
});
</script>

<template>
  <Drawer 
    v-if="drawerOpen"
    :total-price="totalPrice"
    :vat-price="vatPrice"
    @create-order="createOrder"
    :button-disabled="cartButtonDisabled"
  />

  <div class="bg-white w-4/5 m-auto  rounded-r-xl shadow-xl mt-14">
    <Header :total-price="totalPrice" @open-drawer="openDrawer"/>


    <div class="p-10">
      <router-view></router-view>
  
    </div>
  </div>
</template>

