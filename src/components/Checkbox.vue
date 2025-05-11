<template>
  <label class="custom-checkbox">
    <input type="checkbox" :checked="modelValue" @change="onChange" />
    <span class="checkbox-box"></span>
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

<style scoped>
/* Hide the default checkbox */
input[type='checkbox'] {
  display: none;
}

/* Custom checkbox container */
.custom-checkbox {
  margin-left: 0.5rem;
  display: flex;
  align-items: center;
  cursor: pointer;
  font-size: calc(var(--fs-sm) * 0.6); /* Adjust label font size */
}

/* Custom checkbox box */
.checkbox-box {
  width: 20px; /* Adjust size */
  height: 20px; /* Adjust size */
  border: 2px solid var(--text-accent-color); /* Border color */
  border-radius: 4px; /* Rounded corners */
  background-color: white; /* Background color */
  display: inline-block;
  margin-right: 0.5rem; /* Space between box and label */
  position: relative;
}

/* Checked state */
input[type='checkbox']:checked + .checkbox-box {
  background-color: var(--text-accent-color); /* Checked background color */
  border-color: var(--text-accent-color); /* Checked border color */
}

/* Add a checkmark */
input[type='checkbox']:checked + .checkbox-box::after {
  content: '';
  position: absolute;
  top: 4px;
  left: 7px;
  width: 5px;
  height: 10px;
  border: solid white;
  border-width: 0 2px 2px 0;
  transform: rotate(45deg);
}

@media screen and (max-width: 1000px) {
  .custom-checkbox {
    font-size: 1rem; /* Increase label font size */
    gap: 0.5rem; /* Add spacing between checkbox and label */
  }

  .checkbox-box {
    width: 25px; /* Increase checkbox size */
    height: 25px;
  }
}
</style>
