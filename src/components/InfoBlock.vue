<script setup>
    defineProps({
        title: String,
        description: String,
        imageUrl: String
    })
</script>
<template>
    <div class="flex flex-col item-center text-center w-72 mx-auto">
        <img class="mx-auto" height="90" width="90" :src="imageUrl" alt="info image"/>
        <h2 class="mt-4 text-2xl font-medium">{{ title }}</h2>
        <p class="text-gray-400 mt-2">{{ description }}</p>
    </div>
</template>