<template>
  <div class="timer-container">
    <!-- Settings Section -->
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

    <!-- Timer Display -->
    <div class="display">
      <p class="time">{{ formattedTime }}</p>
    </div>

    <!-- Todo List Section -->
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

// Timer state
const workTime = ref(25);
const restTime = ref(5);
const remainingTime = ref(workTime.value * 60); // en secondes
const isRunning = ref(false);
let timerInterval = null;

// Timer functions
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
      //Run sound
      const audio = new Audio('src/assets/sounds/pomodoro.mp3');
      audio.volume = 0.5; //Reducing volume to 50%
      audio.currentTime = 1; // Start from 1 second to avoid the initial silence
      audio.play();

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

/* Layout containers */
div.timer-container {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}

div.settings {
  width: 30%;
  margin-left: 0.5rem;
  margin-top: 0.5rem;
  border-radius: 1rem;
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

div.todosTasks {
  width: 30%;
  margin-left: 0.5rem;
  border-radius: 1rem;
  padding: 0.5rem;
  background-color: #240046;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
}

/* Input styling */
div.inputUser {
  display: flex;
  flex-direction: row;
  justify-content: center;
  gap: 1rem;
  padding-top: 0.5rem;
}

input {
  text-align: center;
  margin-top: 0.1rem;
  justify-content: center;
  border: 3px solid #e0aaff;
  border-radius: 0.5rem;
}

/* Hides spin buttons on number inputs */
input[type='number']::-webkit-inner-spin-button {
  -webkit-appearance: none;
}

/* Button styling */
div.btns {
  display: flex;
  flex-direction: row;
  justify-content: center;
  gap: 0.5rem;
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

/* Timer display */
p.time {
  font-size: var(--fs-xxl);
}

/* Responsive styles */
@media screen and (max-width: 1000px) {
  div.timer-container {
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  div.settings,
  div.display,
  div.todosTasks {
    width: 90%;
    margin-left: 0;
    margin-top: 1rem;
  }

  div.inputUser {
    flex-direction: column; /* Stack the labels vertically */
    gap: 1rem; /* Add spacing between the labels */
    align-items: center; /* Center-align the labels */
  }

  div.btns {
    flex-direction: column;
    gap: 0.5rem;
  }

  button {
    font-size: 0.8rem;
    padding: 0.5rem 1rem;
  }

  input {
    width: 80%;
    font-size: 1rem;
  }

  p.time {
    font-size: var(--fs-xl);
  }

  label {
    display: flex;
    flex-direction: column; /* Stack the <p> and <input> vertically */
    align-items: center; /* Center-align the content inside the label */
    text-align: center; /* Center-align the text */
  }
}
</style>
