<template>
  <label>
    <input type="checkbox" :checked="modelValue" @change="onChange" />
    {{ label }}
  </label>
</template>

<script setup>
defineOptions({
  name: 'CheckboxForTodo',
});

defineProps({
  modelValue: {
    type: Boolean,
    required: true,
  },
  label: {
    type: String,
    required: true,
  },
});

const emits = defineEmits(['update:modelValue']);

const onChange = (event) => {
  emits('update:modelValue', event.target.checked);
};
</script>

<style></style>
