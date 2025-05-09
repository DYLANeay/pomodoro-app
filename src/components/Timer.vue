<template>
  <div class="timer-container">
    <div class="settings">
      <div class="inputUser">
        <label for="">
          <p>Work time</p>
          <input type="number" min="1" max="60" />
        </label>
        <label for="">
          <p>Rest time</p>
          <input type="number" min="1" max="60" />
        </label>
      </div>
      <div class="btn">
        <button>Submit</button>
      </div>
    </div>

    <div class="display"></div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
defineOptions({
  name: 'PomodoroTimer',
});

const workTime = ref(25);
const restTime = ref(5);

// À compléter: logique du timer et formatage de l'affichage
</script>

<style scoped>
@import '@/assets/styles/variables.css';

div.timer-container {
  width: 25%;
  margin-left: 0.5rem;
  margin-top: 0.5rem;
  border-radius: 1rem;
  background-color: rgb(61, 61, 61);
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
}

/* hides shitty things */
input[type='number']::-webkit-inner-spin-button {
  -webkit-appearance: none;
}

input {
  text-align: center;
  margin-top: 0.2rem;
  justify-content: center;
  border: 3px solid rgb(0, 0, 0);
  border-radius: 0.5rem;
}

div.inputUser {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  padding: 0.5rem;
  padding-top: 1rem;
}

div.btn {
  display: flex;
  justify-content: center;
  padding: 0.5rem;
}

button {
  background-color: rgb(0, 0, 0);
  border-radius: 0.5rem;
  padding: 0.26rem;
  font-size: 0.5rem;
  cursor: pointer;
}

button:hover {
  background-color: rgb(0, 0, 0, 0.5);
}
</style>
