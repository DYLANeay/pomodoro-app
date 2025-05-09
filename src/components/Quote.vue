<template>
  <div class="quote center">
    <p class="text-center">{{ quote }}</p>
    <p class="author">
      <strong
        ><i>- {{ author }}</i></strong
      >
    </p>
  </div>
</template>
<script setup>
import { onMounted, ref } from 'vue';

defineOptions({
  name: 'MotivationalQuote',
});

let quote = ref('');
let author = ref('');

//get the quotes and authors from the API
onMounted(async () => {
  const response = await fetch(
    'https://raw.githubusercontent.com/AtaGowani/daily-motivation/refs/heads/master/src/data/quotes.json',
  );

  const data = await response.json();
  const randomIndex = Math.floor(Math.random() * data.length); // get a random index
  // get a random quote and author
  quote.value = data[randomIndex].quote;
  author.value = data[randomIndex].author;
});
</script>

<style>
.quote {
  padding: 0.5rem;
  margin: 20px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border-radius: 1rem;
}

.author {
  font-size: 0.5rem;
  font-weight: light;
  justify-self: right;
}
</style>
