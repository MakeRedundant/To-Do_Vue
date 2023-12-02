<template>
  <div id="app">
    <!-- The main container of our app -->
    <h1>My Todo App</h1>

    <!-- Input field for adding new todos -->
    <input v-model="newTodo" @keyup.enter="addTodo" placeholder="Add new todo" />

    <!-- Component responsible for displaying the list of todos -->
    <TodoList :todos="todos" @remove="removeTodo" />
  </div>
</template>

<!--template section contains the HTML structure of the component.--->
<!--<input v-model="newTodo" @keyup.enter="addTodo" placeholder="Add new todo" /> 
  is an input field bound to the newTodo variable using v-model. -->
<!--<TodoList :todos="todos" @remove="removeTodo" /> is our TodoList component. 
  It receives the todos array as a prop and listens for the remove event to handle todo removal.-->

<script lang="ts">
import { defineComponent, ref } from 'vue';
import TodoList from "./components/TodoList.vue";

export default defineComponent({
  name: 'App',
  components: {
    TodoList,
  },
  setup() {
    const newTodo = ref('');
    const todos = ref<string[]>([]);

    function addTodo() {
      if (newTodo.value.trim() !== '') {
        todos.value.push(newTodo.value);
        newTodo.value = '';
      }
    }

    function removeTodo(index: number) {
      todos.value.splice(index, 1);
    }

    return {
      newTodo,
      todos,
      addTodo,
      removeTodo,
    };
  },
});
</script>

<!--The <script> section contains the logic of the component written in TypeScript.
defineComponent is used to define the Vue component.
import statements bring in necessary functionality. For instance, we import TodoList from its file.
setup() is a composition API function where we define our component's reactive data and methods.
ref() creates reactive variables (newTodo and todos) that can trigger reactivity when their values change.
addTodo() is a function that adds a new todo to the todos array if the input isn't empty.
removeTodo() is a function that removes a todo item from the todos array based on its index.-->