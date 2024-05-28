<script setup>
import {reactive, watch, onMounted, ref} from 'vue'
import CardList from '../components/CardList.vue'
import axios from 'axios';
import { inject } from 'vue';
import debounce from 'lodash.debounce';

const {cart, addToCart, removeFromCart} = inject('cart')
const items = ref([])
const filters = reactive({
    sortBy: 'title',
    searchQuery: '',
});

const onClickAddPlus = (item) => {
    if(!item.isAdded){
        addToCart(item)
    }else{
        removeFromCart(item)
    }
    console.log(cart)
}
const onChangeSelect = (event)=>{
        filters.sortBy = event.target.value
        console.log(event.target.value)
    }
const onChangeSearchInput = debounce ((event)=>{
  filters.searchQuery = event.target.value
  }, 300);
const addToFavorite = async (item) => {
    try{
      if(!item.isFavorite){
        const obj = {
        parentId: item.id,
        item
        }
        const {data} =  await axios.post(`https://e871d9708578b5c7.mokky.dev/favorites`, obj)
        item.isFavorite = true
        item.favoriteId = data.id;
        console.log(data)
      }else{
        await axios.delete(`https://e871d9708578b5c7.mokky.dev/favorites/${item.favoriteId}`)
        item.isFavorite = false;
        delete item.favoriteId;
      }
    }catch(err){
      console.log(err)
    }
    }
const fetchFavorites = async () =>{
    try{
      const { data: favorites } = await axios.get(`https://e871d9708578b5c7.mokky.dev/favorites`)

      items.value = items.value.map(item => {
        const favorite = favorites.find(favorite => favorite.parentId === item.id);

        if(!favorite){
          return item;
        }

        return {
          ...item,
          isFavorite: true,
          favoriteId: favorite.id,
        };
      });
    }catch(err){
      console.log(err)
    }
    }
const fetchItems= async()=>{
    try{
      const params = {
        sortBy: filters.sortBy
      }
      if(filters.searchQuery){
        params.title = `*${filters.searchQuery}*`
      }

      const { data } = await axios.get(`https://604781a0efa572c1.mokky.dev/items`, {
        params
      })
      items.value = data.map((obj) =>({
        ...obj,
        isFavorite: false,
        isAdded: false,
        favoriteId: null
      }))
    }catch(err){
      console.log(err)
    }
    }

onMounted(async () => {
    const localCart = localStorage.getItem('cart')
    cart.value = localCart ? JSON.parse(localCart) : []
    if (!Array.isArray(cart.value)) {
        console.error('Ошибка: данные корзины не являются массивом', cart.value);
        cart.value = []; // Сброс к пустому массиву, если данные некорректны
    }
    await fetchItems();
    await fetchFavorites();

    items.value = items.value.map((item)=>({
        ...item,
        isAdded: cart.value.some((cartItem)=> cartItem.id === item.id)
    }))
})
watch(filters,fetchItems)

watch(cart, ()=> {
    items.value = items.value.map((item) => ({
        ...item,
        isAdded: false
    }))
})

</script>
<template>
    <div>
        <div class="flex justify-between item-center">
            <h2 class="text-3xl font-bold">Все кроссовки</h2>
            <div class="flex gap-4">
                <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
                <option value="name">By Name</option>
                <option value="price">By Price(Cheaper)</option>
                <option value="-price">By Price(Expensive)</option>
                </select>
                <div class="relative">
                    <img class="absolute left-4 top-3"
                    src="/search.svg"
                    />
                    <input @input="onChangeSearchInput"
                    class="border rounded-md py-2 pl-10 pr-4 outline-none focus:border-gray-400"
                    type="text"
                    placeholder="Search"
                    />
                </div>
            </div>
        </div>
        <CardList :items="items" @addToFavorite = "addToFavorite" @add-to-cart="onClickAddPlus"/>
    </div>
</template>