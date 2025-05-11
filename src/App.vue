<template>
  <h1 class="text-center welcomeText">Welcome Home</h1>
  <Quote />
  <div class="components-container">
    <Timer />
    <Footer></Footer>
  </div>
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

@media screen {
  @media (max-width: 1000px) {
    .components-container {
      display: flex;
      flex-direction: row;
      align-items: center;
    }
  }
}

@media screen and (max-width: 1000px) {
  .components-container {
    flex-direction: column; /* Stack components vertically */
    align-items: center;
    gap: 1rem; /* Add spacing between components */
  }

  .welcomeText {
    font-size: 1.5rem; /* Adjust heading size */
    text-align: center; /* Center-align the heading */
  }
}
</style>
