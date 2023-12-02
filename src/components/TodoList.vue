<template>
  <!-- Rendering the list of todos -->
  <ul>
    <!-- Looping through each todo item -->
    <li v-for="(todo, index) in todos" :key="index">
      <!-- Displaying the todo item -->
      {{ todo }}
      
      <!-- Button to remove the corresponding todo item -->
      <button @click="remove(index)">❌</button>
    </li>
  </ul>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';

export default defineComponent({
  name: 'TodoList',
  props: {
    // Define the prop for todos, which expects an array of strings
    todos: {
      type: Array as PropType<string[]>,
      required: true,
    },
  },
  // Define emitted events to communicate with the parent component
  emits: ['remove'],
  methods: {
    // Method to handle removing a todo item by emitting the 'remove' event
    remove(index: number) {
      this.$emit('remove', index); // Emit the 'remove' event with the index
    },
  },
});
</script>
