<template>
  <div class="timer-container">
    <div class="settings">
      <div class="inputUser">
        <label>
          <p>Work time</p>
          <input type="number" min="1" max="60" v-model="workTime" @change="updateRemainingTime" />
        </label>
        <label>
          <p>Rest time</p>
          <input type="number" min="1" max="60" v-model="restTime" />
        </label>
      </div>
      <div class="btns">
        <div class="btn">
          <button @click="isRunning ? stopTimer() : startTimer()">
            {{ isRunning ? 'Stop' : 'Start' }}
          </button>
        </div>
        <div class="btn">
          <button @click="resetTimer()">Reset</button>
        </div>
      </div>
    </div>
    <div class="display">
      <p class="time">{{ formattedTime }}</p>
    </div>
    <div class="todosTasks">
      <Todo />
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onBeforeUnmount } from 'vue';
import Todo from './Todo.vue';

defineOptions({
  name: 'PomodoroTimer',
});

const workTime = ref(25);
const restTime = ref(5);
const remainingTime = ref(workTime.value * 60); // en secondes
const isRunning = ref(false);
let timerInterval = null;

const updateRemainingTime = () => {
  if (!isRunning.value) {
    remainingTime.value = workTime.value * 60;
  }
};

const startTimer = () => {
  isRunning.value = true;
  timerInterval = setInterval(() => {
    if (remainingTime.value > 0) {
      remainingTime.value--;
    } else {
      clearInterval(timerInterval);
      isRunning.value = false;
      // Switch to rest time
      remainingTime.value = restTime.value * 60;
    }
  }, 1000);
};

const stopTimer = () => {
  isRunning.value = false;
  clearInterval(timerInterval);
};

const resetTimer = () => {
  isRunning.value = false;
  clearInterval(timerInterval);
  remainingTime.value = workTime.value * 60; // Reset to work time
};

// Format the time to have smthg like 00:00
const formattedTime = computed(() => {
  const minutes = Math.floor(remainingTime.value / 60);
  const seconds = remainingTime.value % 60;
  const formattedMinutes = String(minutes).padStart(2, '0');
  const formattedSeconds = String(seconds).padStart(2, '0');
  return `${formattedMinutes}:${formattedSeconds}`;
});

// Nettoyage de l'intervalle lors de la destruction du composant
onBeforeUnmount(() => {
  clearInterval(timerInterval);
});
</script>

<style scoped>
@import '@/assets/styles/variables.css';

div.settings {
  width: 30%;
  margin-left: 0.5rem;
  margin-top: 0.5rem;
  border-radius: 1rem;
  background-color: #240046;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
}

div.btns {
  display: flex;
  flex-direction: row;
  justify-content: center;
  /* padding: 0.5rem; */
  gap: 0.5rem;
}

div.todosTasks {
  width: 30%;
  margin-left: 0.5rem;
  border-radius: 1rem;
  padding: 0.5rem;
  background-color: #240046;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
}

div.display {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 30%;
  margin-left: 0.5rem;
  margin-top: 0.5rem;
  border-radius: 1rem;
  background-color: #240046;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
}

div.timer-container {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}

/* hides shitty things */
input[type='number']::-webkit-inner-spin-button {
  -webkit-appearance: none;
}

input {
  text-align: center;
  margin-top: 0.1rem;
  justify-content: center;
  border: 3px solid #e0aaff;
  border-radius: 0.5rem;
}

div.inputUser {
  display: flex;
  flex-direction: row;
  justify-content: center;
  gap: 1rem;
  padding-top: 0.5rem;
}

div.btn {
  display: flex;
  justify-content: center;
  padding: 0.5rem;
}

button {
  background-color: #7b2cbf;
  border-radius: 0.5rem;
  padding: 0.26rem;
  font-size: 0.5rem;
  cursor: pointer;
}

button:hover {
  background-color: rgba(0, 0, 0, 0.5);
}

p.time {
  font-size: var(--fs-xxl);
}
</style>
