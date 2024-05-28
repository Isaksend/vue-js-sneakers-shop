
<script setup>
  import { onMounted,ref, provide, watch, computed } from 'vue';
  
  import Header from './components/Header.vue'
  import Drawer from "./components/Drawer.vue"
/* Cart start */
  const cart = ref([])
  const drawerOpen = ref(false);
  const totalPrice = computed(
    () => cart.value.reduce((acc, item) => acc + item.price, 0)
  );
  const vatPrice = computed(()=> Math.round((totalPrice.value * 5)/100));
  const closeDrawer = () =>{
    drawerOpen.value = false
  }
  const openDrawer = () =>{
    drawerOpen.value = true
  }
  const addToCart = (item) => {
    cart.value.push(item)
    item.isAdded = true
  }
  const removeFromCart = (item) => {
    const index = cart.value.findIndex(cartItem => cartItem.id === item.id);
    if (index !== -1) {
      cart.value.splice(index, 1);
      item.isAdded = false;
    }
  }; 
  watch(
    cart, 
    ()=>{
      localStorage.setItem('cart', JSON.stringify(cart.value))
    },
    {
      deep: true
    }
  )
  provide('cart',{
    cart,
    closeDrawer,
    openDrawer,
    addToCart,
    removeFromCart,
  } )

/* Cart end*/

</script>
<template>
  <div>
    <Drawer 
      v-if="drawerOpen" 
      :total-price="totalPrice" 
      :vatPrice="vatPrice" 
    />
    <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
      <Header :total-price="totalPrice" @openDrawer="openDrawer" />
      <div class="p-10">
        <router-view>
        </router-view>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>