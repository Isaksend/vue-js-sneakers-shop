<script setup>
import Card from "./Card.vue"
import { defineProps } from 'vue'
import { defineEmits } from "vue"
const props = defineProps({
    items: {
        type: Array,
        required: true,
    },
    isFavorites: Boolean
})
const emit = defineEmits(['addToFavorite', 'addToCart'])

const onClickFavorite = (item) => {
    emit('addToFavorite', item)
}
</script>

<template>
    <div class="grid grid-cols-3 gap-5" v-auto-animate>
        <Card 
            v-for="item in props.items"
            :key="item.id"
            :id="item.id"
            :title="item.title"
            :imageUrl="item.imageUrl"
            :price="item.price"
            :onClickAdd="isFavorites ? null : () => emit('addToCart',item)"
            :onClickFavorite="isFavorites ? null : () =>emit('addToFavorite', item)"
            :isFavorite="item.isFavorite"
            :isAdded="item.isAdded"
        />
    </div>
</template>
