<template>
  <!-- Rendering the list of todos -->
  <ul>
    <!-- Looping through each todo item -->
    <li v-for="(todo, index) in todos" :key="index" :draggable="true" @dragstart="onDragStart($event, index)" @dragover.prevent @drop="onDrop($event, index)">
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
    // Method to initiate drag when a todo item is dragged
    onDragStart(event: DragEvent, index: number) {
      event.dataTransfer?.setData('text/plain', index.toString());
    },
    // Method to handle dropping todos and reordering the list
    onDrop(event: DragEvent, newIndex: number) {
      const oldIndex = parseInt(event.dataTransfer?.getData('text/plain') || '-1');
      if (oldIndex >= 0 && oldIndex !== newIndex) {
        const todoToMove = this.todos[oldIndex];
        this.todos.splice(oldIndex, 1);
        this.todos.splice(newIndex, 0, todoToMove);
      }
    },
  },
});
</script>

