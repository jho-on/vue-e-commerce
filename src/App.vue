<template>
    <Header />
    <RouterView/>
</template>



<script setup>
    import { RouterView } from 'vue-router';
    import Header from './components/Header.vue';
    import './assets/main.scss';
    import {ref} from "vue";
    const cart = ref();

    function handleAddCart(product){
        console.log(product)
    }
</script>


