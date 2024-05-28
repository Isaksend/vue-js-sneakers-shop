<script setup>
import {ref, onMounted} from 'vue'
import CardList  from '../components/CardList.vue'
import axios from 'axios';
const favorites = ref([])
onMounted(async()=> {
    try{
        const {data} = await axios.get('https://e871d9708578b5c7.mokky.dev/favorites')
        favorites.value = data.map((obj) => obj.item)
    }catch(err){
        console.log(err)
    }
})

</script>
<template>
    <div>
        <h1>My Favorites</h1>
        <CardList :items="favorites" is-favorites/>
    </div>
</template>