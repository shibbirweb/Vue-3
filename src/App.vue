<template>
  <h1>Vue 3 Todo App</h1>
  <form @submit.prevent="addNewTodo">
    <label>New Todo</label>
    <input v-model="newTodo" name="newTodo" />
    <button>Add New Todo</button>
  </form>
  <button @click="markAllDone">Mark all done</button>
  <button @click="removeAllDone">Remove all done</button>
  <button @click="removeAllTodo">Remove all todos</button>
  <ul>
    <li v-for="(todo, index) in todos" :key="todo.id">
      <h3 :class="{ done: todo.done }" class="todo" @click="toggleDone(todo)">
        {{ todo.content }}
      </h3>
      <button @click="removeTodo(index)">Remove todo</button>
    </li>
  </ul>
</template>

<script>
import { ref } from "vue";
export default {
  name: "App",
  setup() {
    const newTodo = ref("");
    const todos = ref([]);

    function addNewTodo() {
      todos.value.push({
        id: Date.now(),
        done: false,
        content: newTodo.value,
      });

      // clear input
      newTodo.value = "";
    }

    function toggleDone(todo) {
      todo.done = !todo.done;
    }

    function removeTodo(index) {
      todos.value.splice(index, 1);
    }

    function markAllDone() {
      todos.value.forEach((todo) => (todo.done = true));
    }

    function removeAllTodo() {
      todos.value = [];
    }

    function removeAllDone() {
      todos.value = todos.value.filter((todo) => !todo.done);
    }

    return {
      addNewTodo,
      newTodo,
      todos,
      toggleDone,
      removeTodo,
      markAllDone,
      removeAllTodo,
      removeAllDone,
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.todo {
  cursor: pointer;
}
.done {
  text-decoration: line-through;
}
</style>
