<script setup>
import { ref } from "vue";
import TodoItem from "../components/TodoItem.vue";

let id = 0;
const todos = ref([]);
const titleClass = ref("title");
const newTodo = ref("");

function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value, done: false });
  newTodo.value = '';
}
</script>

<template>
  <form @submit.prevent="addTodo">
    <h1 :class="titleClass">ToDo(やることリスト)</h1>
    <input v-model="newTodo" required placeholder="タスクを書いてね" />
    <button type="submit">追加</button>
  </form>
  <!-- `todos` を `TodoItem` に渡す -->
  <TodoItem :todos="todos" />
</template>

<style scoped>
.title {
  color: orange;
}
</style>
