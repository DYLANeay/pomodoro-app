<template>
  <h1 class="text-center welcomeText">Welcome Home</h1>
  <Quote />
</template>

<script setup>
// import { ref } from 'vue'
import Quote from './components/Quote.vue';
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
