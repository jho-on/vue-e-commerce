<template>
    <Header />
    <RouterView @addCart="handleAddCart" :productList="cart"/>
</template>



<script setup>
    import { RouterView } from 'vue-router';
    import Header from './components/Header.vue';
    import './assets/main.scss';
    import {ref} from "vue";

    const cart = ref([]);

    function handleAddCart(product){
        if(!cart.value.includes(product)){
            cart.value.push(product)
        }
    }
</script>


