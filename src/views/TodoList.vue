<script setup>
import { ref } from "vue";
import TodoItem from "../components/TodoItem.vue";
import axios from 'axios';


const apiUrl = 'http://localhost:8080/api/todos';
const todos = ref([]);
const titleClass = ref("title");
const newTodo = ref("");

// 既存のToDoを取得
async function fetchTodos() {
  try {
    const response = await axios.get(apiUrl);
    todos.value = response.data;
  } catch (error) {
    console.error('Error fetching todos:', error);
  }
}

// 新しいToDoを追加
async function addTodo() {
  try {
    const response = await axios.post(apiUrl, {
      text: newTodo.value,
      done: false
    });
    todos.value.push(response.data);
    newTodo.value = '';
  } catch (error) {
    console.error('Error adding todo:', error);
  }
}

function updateTodos(updatedTodos) {
  todos.value = updatedTodos;
}

fetchTodos();
</script>

<template>
  <form @submit.prevent="addTodo">
    <h1 :class="titleClass">ToDo(やることリスト)</h1>
    <input v-model="newTodo" required placeholder="タスクを書いてね" class="input-box" />
    <button type="submit" class="submit-button">追加</button>
  </form>
  <TodoItem :todos="todos" @update:todos="updateTodos" />
</template>

<style scoped>
.title {
  color: orange;
}

.input-box {
  border: 2px solid orange;
  padding: 5px;
  border-radius: 4px;
}

.submit-button {
  background-color: orange;
  border: 2px solid orange;
  color: white;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer;
}

.submit-button:hover {
  background-color: white;
  color: orange;
}
</style>
