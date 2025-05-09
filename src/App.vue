<template>
  <h1 class="text-center welcomeText">Welcome Home</h1>
  <Quote />
  <Timer />
  <Footer></Footer>
</template>

<script setup>
// import { ref } from 'vue'
import Quote from './components/Quote.vue';
import Footer from './components/Footer.vue';
import Timer from './components/Timer.vue';
</script>

<style>
@import 'tailwindcss';
@import '@/assets/styles/global.css';

.welcomeText {
  font-size: 2rem;
  font-weight: bold;
  margin-top: 30px;
  margin-bottom: 20px;
}
</style>
