<script setup>
import Header from './components/Header.vue';
import CardList from './components/CardList.vue';
import Drawer from './components/Drawer.vue';
import axios from 'axios';
import { onMounted, provide, reactive, ref, watch } from 'vue';


const items = ref([]);

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})



const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}
const onChangeSearchInput = (event) =>{
  filters.searchQuery = event.target.value
}

const fetchFavorites = async () => {
  try{
    const {data: favorites} = await axios.get('https://007c7bb5747041ef.mokky.dev/favorites')

    items.value = items.value.map(item => {
      const favorite = favorites.find(favorite => favorite.parentId === item.id);
      
      if(!favorite){
        return item;
      }

      return {
        ...item,
        isFavorite:true,
        favoriteId: favorite.id
      };
    });

    console.log(items.value)
  }catch (err){
    console.log(err);
  }
}

const addToFavorite = async (item) => {
  try{ 
    if (!item.isFavorite){

      const obj = {parentId: item.id};

      item.isFavorite = true;


      const {data} = await axios.post('https://007c7bb5747041ef.mokky.dev/favorites', obj);

      item.favoriteId = data.id;
    }
    else{
      await axios.delete(`https://007c7bb5747041ef.mokky.dev/favorites/${item.favoriteId}`)
      item.isFavorite = false;
      item.favoriteId = null;
    }
  
    console.log(data);

    
  }catch(err){
    console.log(err);
  }

}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy,
      // searchQuery: filters.searchQuery
    }

    if(filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`;
    }


    const { data } = await axios.get(`https://604781a0efa572c1.mokky.dev/items`,{
      params
    });
    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId:null,
      isAdded: false
    }));
  }catch (err) {
    console.log(err);
  }
}

onMounted(async () => {
  await fetchItems();
  await fetchFavorites();
});

watch(filters, fetchItems);


provide('addToFavorite',addToFavorite);
</script>

<template>
  <!-- <Drawer/> -->
  <div class="bg-white w-4/5 m-auto  rounded-r-xl shadow-xl mt-14">
    <Header/>


    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>
        
        <div class="flex gap-4">
          <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
            <option value="name">По названию</option>
            <option value="price">По цене (дешевые)</option>
            <option value="-price">По цене (дорогие)</option>

          </select>

          <div class="relative">
            <img class="absolute left-4 top-3" src="/search.svg" >
            <input @input="onChangeSearchInput" class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400" type="text" placeholder="Поиск...">
          </div>
        </div>
        </div>

      <div class="mt-10">
        <CardList :items="items" @addToFavorite="addToFavorite"/>
      </div>
  
    </div>
  </div>
</template>

