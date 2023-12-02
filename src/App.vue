<template>
  <div id="app">
    <!-- The main container of our app -->
    <h1>My Todo App</h1>

    <!-- Input field for adding new todos -->
    <div style="display: flex; align-items: center;">
      <input v-model="newTodo" @keyup.enter="addTodo" placeholder="Add new todo" style="flex: 1; margin-right: 5px;" />
      <button @click="addTodo" style="font-size: 1.0em;">✔️</button>
    </div>

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
import { defineComponent, ref, onMounted } from 'vue';
//import from vue, defineComponent, ref: ref , onMounted
/*
defineComponent: This is a function provided by Vue 3's Composition API. It's used to define a new component.

ref: ref is a Vue 3 Composition API function used to create reactive references to a value.

onMounted: onMounted is a Vue 3 Composition API hook that runs a given function when the component is mounted to the DOM.

*/
import TodoList from "./components/TodoList.vue";

export default defineComponent({
  name: 'App', // Name of the component

  components: {
    TodoList, // Importing the TodoList component to use in this component
  },

  setup() {
    // Using the Composition API setup() function to define reactive data and methods

    // Creating reactive variables to store data
    const newTodo = ref(''); // Represents the input field for adding new todos
    const todos = ref<string[]>([]); // Stores the list of todos as an array of strings
    
    // Load todos from local storage when the component is mounted
    onMounted(() => {
  // Retrieving todos from local storage when the component is mounted

  // Retrieve the 'todos' data from local storage
  const storedTodos = localStorage.getItem('todos');

  // If there are stored todos in local storage, update the todos variable
  if (storedTodos) {
    // Parse the retrieved data from local storage (stored as a string) back to an array
    todos.value = JSON.parse(storedTodos);
  }
});


    // Function to add a new todo to the todos array
    function addTodo() {
      if (newTodo.value.trim() !== '') {
        todos.value.push(newTodo.value); // Pushes the new todo into the todos array
        newTodo.value = ''; // Clears the input field after adding the todo
        saveTodosToLocalStorage(); 
      }
    }

    // Function to remove a todo item from the todos array based on its index
    function removeTodo(index: number) {
      todos.value.splice(index, 1); // Removes the todo at the specified index from the todos array
      saveTodosToLocalStorage();
    }

    function saveTodosToLocalStorage() {
      localStorage.setItem('todos', JSON.stringify(todos.value));
    }

    // Returning data and methods that can be accessed in the template
    return {
      newTodo, // The input value for adding new todos
      todos, // The array containing all the todos
      addTodo, // Function to add a new todo
      removeTodo, // Function to remove a todo
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