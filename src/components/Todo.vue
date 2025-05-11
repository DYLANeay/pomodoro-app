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

import { ref, computed, watch, onMounted } from 'vue';
import Checkbox from './Checkbox.vue';

// Reactive variables
const newTodo = ref('');
const todos = ref([]);

// Load todos from localStorage on component mount if there's any
onMounted(() => {
  if (localStorage.getItem('todos')) {
    todos.value = JSON.parse(localStorage.getItem('todos'));
  }
});

// Watch for changes in todos and save automatically, it changes ALL the localStorage stored values, even if only one changes
watch(
  todos,
  (newTodos) => {
    localStorage.setItem('todos', JSON.stringify(newTodos));
  },
  { deep: true },
);

// Computed property to sort todos
const sortedTodos = computed(() => {
  return [...todos.value].sort((a, b) => a.completed - b.completed);
});

// Add a new todo
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

// Modify an existing todo
const modifyTasks = (id) => {
  const todo = todos.value.find((todo) => todo.id === id); //Value obligatory because of the fact that todos is a ref
  // Find the todo with the given id
  if (todo) {
    const newTitle = prompt('Enter new task title:', todo.title);
    if (newTitle !== null && newTitle.trim() !== '') {
      todo.title = newTitle.trim();
    }
  }
};

// Reset all todos
const resetTasks = () => {
  if (confirm('Are you sure you want to reset all tasks?')) {
    todos.value = [];
    localStorage.removeItem('todos');
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
