<template>
  <form @submit.prevent="addNewTodo">
    <label>Add new todo</label>
    <input v-model="newTodo" name="newTodo" />
    <button>Add</button>
  </form>
  <button @click="markAllDone">Mark all done</button>
  <button @click="removeAllDone">Remove all done</button>
  <button @click="removeAll">Remove all</button>

  <ul>
    <li v-for="(todo, index) in todos" :key="index">
      <h4 :class="{ done: todo.done }" class="todo" @click="toggleDone(todo)">
        {{ todo.content }}
      </h4>
      <button @click="removeTodo(index)">Remove</button>
    </li>
  </ul>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";

interface Todo {
  id: number;
  done: boolean;
  content: string;
}

export default defineComponent({
  name: "App",
  setup() {
    const newTodo = ref<string>("");
    const todos = ref<Array<Todo>>([]);

    function addNewTodo(): void {
      todos.value.push({
        id: Date.now(),
        content: newTodo.value,
        done: false,
      });
      // clear input
      newTodo.value = "";
    }

    function toggleDone(todo: Todo): void {
      todo.done = !todo.done;
    }

    function removeTodo(index: number): void {
      todos.value.splice(index, 1);
    }

    function markAllDone(): void {
      todos.value.forEach((todo: Todo) => (todo.done = true));
    }

    function removeAll(): void {
      todos.value = [];
    }

    function removeAllDone(): void {
      todos.value = todos.value.filter((todo) => !todo.done);
    }
    return {
      addNewTodo,
      newTodo,
      todos,
      toggleDone,
      removeTodo,
      markAllDone,
      removeAll,
      removeAllDone,
    };
  },
});
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
