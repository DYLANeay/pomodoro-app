<template>
  <form action="" @submit.prevent="addTodo">
    <fieldset>
      <input v-model="newTodo" type="text" placeholder="Ajouter une tâche" />
      <button class="add-button" :disabled="!newTodo.trim()" type="submit">Ajouter</button>
    </fieldset>
  </form>
  <ul>
    <li v-for="todo in sortedTodos" :key="todo.id" :class="{ completed: todo.completed }">
      <Checkbox :label="todo.title" v-model="todo.completed" />
    </li>
  </ul>
</template>

<script setup>
defineOptions({
  name: 'TodoList',
});

import { ref, computed } from 'vue';
import Checkbox from './Checkbox.vue';

const newTodo = ref('');
const todos = ref([]);

const sortedTodos = computed(() => {
  const sortedTodo = [...todos.value].sort((a, b) => a.completed - b.completed);
  return sortedTodo;
});

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
</script>

<style>
@import '@/assets/styles/variables.css';

.completed {
  text-decoration: line-through;
  color: gray;
}

fieldset {
  display: flex;
  justify-content: space-evenly;
  flex-direction: row;
  align-items: center;
}

.add-button {
  font-size: 0.7rem; /* Réduit la taille du texte */
  cursor: pointer;
}

input[type='text'] {
  color: grey;
  font-size: calc(var(--fs-sm) * 0.5);
}

li {
  font-size: calc(var(--fs-sm) * 0.7) !important;
  color: white;
}
</style>
