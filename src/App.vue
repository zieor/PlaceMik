<script setup>
import { ref, provide, onMounted } from 'vue'
import Header from "@/components/Header.vue";
import Footer from "@/components/Footer.vue";


const cartCount = ref(0)

onMounted(() => {

  const saved = localStorage.getItem('cartCount')
  if (saved) {
    cartCount.value = parseInt(saved)
  }
})

const addToCart = (product) => {
  cartCount.value++

  localStorage.setItem('cartCount', cartCount.value)
  console.log('Товар добавлен в корзину:', product)
}


provide('cartCount', cartCount)
provide('addToCart', addToCart)
</script>

<template>
  <header>
    <Header/>
  </header>

  <main>
    <router-view />
  </main>

  <footer>
    <Footer/>
  </footer>
</template>

<style scoped>
</style>