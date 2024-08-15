<script setup>
import { ref } from "vue";
import TodoItem from "../components/TodoItem.vue";
import axios from "axios";

const apiUrl = "http://localhost:8081/api/todos";
const todos = ref([]);
const titleClass = ref("title");
const newTodo = ref({
  title: "",
  deadline: ""
});
const hideCompleted = ref(false);


// 既存のToDoを取得
async function fetchTodos() {
  try {
    const response = await axios.get(apiUrl);
    todos.value = response.data;
  } catch (error) {
    console.error("Error fetching todos:", error);
  }
}

// 新しいToDoを追加
async function addTodo() {
  try {
    const response = await axios.post(apiUrl, {
      title: newTodo.value.title,
      completed: false,
      deadline: newTodo.value.deadline || null
    });
    todos.value.push(response.data);
    newTodo.value = {
      title: "",
      deadline: ""
    };
  } catch (error) {
    console.error("Error adding todo:", error);
  }
}


function updateTodos(updatedTodos) {
  todos.value = updatedTodos;
}

fetchTodos();
</script>

<template>
  <form @submit.prevent="addTodo" class="todo-form">
    <h1 :class="titleClass">ToDo(やることリスト)</h1>
    <div class="input-group">
      <input
        v-model="newTodo.title"
        required
        placeholder="タスクを書いてね"
        class="input-box"
      />
    </div>
    <div class="input-group">
      <input 
        type="date" 
        v-model="newTodo.deadline" 
        placeholder="期限（任意）" 
        class="input-box"
      />
    </div>
    <button type="submit" class="submit-button">追加</button>
   
  </form>
  <TodoItem :todos="todos" :hideCompleted="hideCompleted" @update:todos="updateTodos" />
</template>

<style scoped>
.title {
  color: orange;
}

.todo-form {
  display: flex;
  flex-direction: column;
}

.input-group {
  margin-bottom: 15px; /* フォーム要素間のスペースを追加 */
}

.input-box {
  border: 2px solid orange;
  padding: 5px;
  border-radius: 4px;
  width: 100%; /* 幅を100%に設定して、フォーム全体の幅に合わせる */
}

.submit-button {
  background-color: orange;
  border: 2px solid orange;
  color: white;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer;
  margin-bottom: 10px; /* ボタンの下にスペースを追加 */
}

.submit-button:hover {
  background-color: white;
  color: orange;
}

.toggle-button {
  background-color: orange;
  border: 2px solid orange;
  color: white;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer;
}

.toggle-button:hover {
  background-color: white;
  color: orange;
}
</style>
