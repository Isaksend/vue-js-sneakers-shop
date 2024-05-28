<script setup>
import { defineProps } from 'vue'
const props = defineProps({
    id: Number,
    title: String,
    imageUrl: String,
    price: Number,
    isFavorite: Boolean,
    isAdded: Boolean,
    onClickAdd: Function,
    onClickFavorite: Function,
})



</script>

<template>
    <div class="relative bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer hover:-translate-y-2 hover:shadow-2xl">
        <img 
        v-if="onClickFavorite"
        @click="props.onClickFavorite" :src="!props.isFavorite ? '/like-1.svg':'/like-2.svg'" alt="Like" class="absolute top-8 left-8"/>
        <img :src="props.imageUrl" alt="Sneaker"/>
        <p>{{ props.title }}</p>
        <div class="flex justify-between">
            <div class="flex flex-col">
                <span class="text-slate-400">Price</span>
                <b>{{ props.price }} usd</b>
            </div>
            <img
            v-if="onClickAdd"
            @click="props.onClickAdd" :src="!props.isAdded ? '/plus.svg':'/checked.svg'" alt="Plus" />
        </div>
    </div>
</template>
