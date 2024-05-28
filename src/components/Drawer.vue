<script setup>
    import DrawerHead from "./DrawerHead.vue"
    import CartItemList from "./CartItemList.vue"
    import InfoBlock from "./InfoBlock.vue"
    import { ref, inject, computed } from "vue"
    import axios from "axios"
    const {cart, closeDrawer} = inject('cart');
    const props = defineProps({
        totalPrice: Number,
        vatPrice: Number,
        buttonDisabled: Boolean
    })
    const isCreating = ref(false);
    const orderId = ref(null);
    const createOrder = async() => {
    try{
        isCreating.value = true
        const { data } = await axios.post(`https://e871d9708578b5c7.mokky.dev/orders`, {
        items: cart.value,
        totalPrice: props.totalPrice.value
    })
        cart.value = []
        orderId.value = data.id;
        return data
    }catch(err){
        console.log(err)
    }finally{
        isCreating.value = false
    }
    } 
    const cartIsEmpty = computed(()=> cart.value.length === 0)
    const buttonDisabled = computed(()=>  
    isCreating.value || cartIsEmpty.value
    );
</script>

<template>
    <div>
        <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70">
        </div>
        <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
            <DrawerHead @closeDrawer = "closeDrawer"/>
            <div v-if="!totalPrice || orderId" class="flex h-full items-center">
                <InfoBlock 
                    v-if="!totalPrice && !orderId"
                    title="Cart empty" 
                    description="Add some items for create order" 
                    image-url="./package-icon.png"
                />
                <InfoBlock 
                    v-if="orderId"
                    title="Sneakers on the road!" 
                    :description="`Your order in proccessing, please waiting some minutes. Your order # ${orderId}`"
                    image-url="./order-success-icon.png"
                />
            </div>
            <div v-else>
                <CartItemList />
                <div class="flex flex-col gap-4 my-7">
                    <div class="flex gap-2">
                        <span>
                            Cost:
                        </span>
                        <div class="flex-1 border-b border-dashed"></div>
                        <b>{{ totalPrice }}usd</b>
                    </div>
                    <div class="flex gap-2">
                        <span>
                            Tax:
                        </span>
                        <div class="flex-1 border-b border-dashed"></div>
                        <b>{{ vatPrice }} usd</b>
                    </div>
                    <button 
                        :disabled="buttonDisabled"
                        @click="createOrder"
                        class="transition bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-400 hover:bg-lime-600 active:bg-lime-700 cursor-pointer"> 
                            Payment
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>