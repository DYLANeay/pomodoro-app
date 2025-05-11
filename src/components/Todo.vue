<template>
  <form action="" @submit.prevent="addTodo">
    <fieldset>
      <input v-model="newTodo" type="text" placeholder="Ajouter une tâche" />
      <button class="add-button" :disabled="!newTodo.trim()" type="submit">Add</button>
      <hr />
    </fieldset>
  </form>

  <ul>
    <li v-for="todo in sortedTodos" :key="todo.id" :class="{ completed: todo.completed }">
      <div class="task-container">
        <Checkbox :label="todo.title" v-model="todo.completed" />
        <button @click="modifyTasks(todo.id)" class="modify-btn">modify</button>
      </div>
    </li>
  </ul>

  <div class="resetTasks">
    <button class="resetTasks-btn" @click="resetTasks">Reset</button>
  </div>
</template>

<script setup>
defineOptions({
  name: 'TodoList',
});

import { ref, computed } from 'vue';
import Checkbox from './Checkbox.vue';

// Data
const newTodo = ref('');
const todos = ref([
  { id: 1, title: 'Task 1', completed: false },
  { id: 2, title: 'Task 2', completed: true },
  { id: 3, title: 'Task 3', completed: false },
]);

// Computed properties
const sortedTodos = computed(() => {
  const sortedTodo = [...todos.value].sort((a, b) => a.completed - b.completed);
  return sortedTodo;
});

// Methods
const addTodo = () => {
  const trimmedTodo = newTodo.value.trim();
  if (trimmedTodo) {
    todos.value.push({
      id: Date.now(),
      title: trimmedTodo,
      completed: false,
    });
    newTodo.value = '';
  }
};

const modifyTasks = (id) => {
  const todo = todos.value.find((todo) => todo.id === id);
  if (todo) {
    const newTitle = prompt('Enter new task title:', todo.title);
    if (newTitle !== null && newTitle.trim() !== '') {
      todo.title = newTitle.trim();
    }
  }
};

const resetTasks = () => {
  if (confirm('Are you sure you want to reset all tasks?')) {
    //confirm just opens a dialog box to make sure the user wants to reset
    todos.value = [];
  }
};
</script>

<style>
@import '@/assets/styles/variables.css';

/* Task appearance */
.completed {
  text-decoration: line-through;
  color: #3c096c;
}

.task-container {
  margin-top: 0.1rem;
  display: flex;
  justify-content: space-around;
  align-items: center;
}

li {
  font-size: calc(var(--fs-sm) * 0.7) !important;
  color: #e0aaff;
}

/* Form elements */
fieldset {
  display: flex;
  justify-content: space-evenly;
  flex-direction: row;
  align-items: center;
}

input[type='text'] {
  color: #e0aaff;
  font-size: calc(var(--fs-sm) * 0.5);
}

/* Buttons */
.add-button {
  font-size: 0.7rem; /* Réduit la taille du texte */
  cursor: pointer;
}

.add-button:active {
  transform: scale(0.95);
}

.resetTasks {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 0.2rem;
}

.modify-btn {
  background-color: #7b2cbf;
  padding: 0.3rem;
  border-radius: 1rem;
  font-size: 0.2rem; /* Réduit la taille du texte */
  cursor: pointer;
}

button:hover {
  background-color: #9d4edd;
}

.resetTasks-btn {
  background-color: #7b2cbf;
  padding: 0.4rem;
  border-radius: 1rem;
  font-size: 0.4rem; /* Réduit la taille du texte */
  cursor: pointer;
}

/* Responsive styling */
@media screen and (max-width: 1000px) {
  fieldset {
    flex-direction: column; /* Stack inputs and button vertically */
    gap: 0.5rem;
  }

  .task-container {
    flex-direction: column; /* Stack tasks vertically */
    align-items: flex-start;
    gap: 0.5rem;
  }

  .add-button,
  .modify-btn,
  .resetTasks-btn {
    font-size: 0.9rem; /* Increase button text size for smaller screens */
    padding: 0.4rem 0.8rem; /* Adjust padding */
  }

  input[type='text'] {
    width: 100%; /* Make input take full width */
    font-size: 1rem; /* Increase input text size */
  }

  ul {
    padding: 0;
  }

  li {
    font-size: 1rem; /* Adjust task font size */
  }
}
</style>
