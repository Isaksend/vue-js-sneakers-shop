<script setup>
    const emit = defineEmits(['openDrawer']);
    defineProps({
        totalPrice: Number
    });
</script>
<template>
    <header class="flex justify-between border-b border-slate-300 px-10 py-8">
        <router-link to="/">
            <div class="flex items-center gap-4">
                <img src="/logo.png" alt="Logo" class="w-10"/>
                <div>
                    <h2 class="text-xl font-bold uppercase">
                        Vue sneakers
                    </h2>
                    <p class="text-slate-500 ">
                        Shop for best sneakers
                    </p>
                </div>
            </div>
        </router-link>
        <ul class="flex items-center gap-10">
            <li @click="()=> emit('openDrawer')" class="flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer">
            <img src="/cart.svg" alt="Cart"/>
            <b>{{ totalPrice }} usd</b>
            </li>
            <router-link to="/favorites">
                <li class="flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer">
                    <img src="/heart.svg" alt="Heart"/>
                    <span>Favorites</span>
                </li>
            </router-link>
            <li class="flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer">
            <img src="/profile.svg" alt="Profile"/>
            <span>Profile</span>
            </li>   
        </ul>
    </header>  
</template>
<style scoped>
</style>

