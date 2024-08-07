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
function updateTodos(updatedTodos) {
  todos.value = updatedTodos; // 親コンポーネントの`todos`を更新
}
</script>

<template>
  <form @submit.prevent="addTodo">
    <h1 :class="titleClass">ToDo(やることリスト)</h1>
    <input v-model="newTodo" required placeholder="タスクを書いてね" class="input-box" />
    <button type="submit" class="submit-button">追加</button>
  </form>
  <!-- `todos` を `TodoItem` に渡し、更新イベントを受け取る -->
  <TodoItem :todos="todos" @update:todos="updateTodos" />
</template>

<style scoped>
.title {
  color: orange;
}

.input-box {
  border: 2px solid orange; /* テキストボックスの縁の色をオレンジに設定 */
  padding: 5px; /* テキストボックス内の余白を設定 */
  border-radius: 4px; /* 角を少し丸める */
}

.submit-button {
  background-color: orange;
  border: 2px solid orange;
  color: white; /* テキストの色をオレンジに設定 */
  padding: 5px 10px; /* ボタン内の余白を設定 */
  border-radius: 4px; /* 角を少し丸める */
  cursor: pointer; /* カーソルをポインタに変更 */
}

.submit-button:hover {
  background-color: white; /* ホバー時の背景色をオレンジに変更 */
  color: orange; /* ホバー時のテキスト色を白に変更 */
}
</style>
