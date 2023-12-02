<template>
    <ul>
      <li v-for="(todo, index) in todos" :key="index">
        {{ todo }}
        <button @click="remove(index)">❌</button>
      </li>
    </ul>
  </template>
  
  <script lang="ts">
  import { defineComponent, PropType } from 'vue';
  
  export default defineComponent({
    name: 'TodoList',
    props: {
      todos: {
        type: Array as PropType<string[]>,
        required: true,
      },
    },
    emits: ['remove'],
    methods: {
      remove(index: number) {
        this.$emit('remove', index);
      },
    },
  });
  </script>
  