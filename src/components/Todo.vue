<template>
  <form action="" @submit.prevent="addTodo">
    <fieldset>
      <input v-model="newTodo" type="text" placeholder="Ajouter une tâche" />
      <button :disabled="!newTodo.trim()" type="submit">Ajouter</button>
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
</style>
