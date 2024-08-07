<script setup>
import { ref, computed, defineProps, defineEmits } from "vue";

// `props` で受け取る
const props = defineProps({
  todos: {
    type: Array,
    required: true
  }
});

// 親コンポーネントにイベントを送信するための`emit`関数を定義
const emit = defineEmits(['update:todos']);

const hideCompleted = ref(false);

const filteredTodos = computed(() => {
  return hideCompleted.value ? props.todos.filter((t) => !t.done) : props.todos;
});

function removeTodo(todo) {
  // `emit` を使って親に更新を通知
  const updatedTodos = props.todos.filter((t) => t !== todo);
  emit('update:todos', updatedTodos); // 親コンポーネントに削除後の配列を通知
}
</script>

<template>
  <div>
    <p v-if="todos.length === 0">ToDoがまだありません！</p>
    <ul>
      <li v-for="todo in filteredTodos" :key="todo.id" class="todo-item">
        <input type="checkbox" v-model="todo.done" class="checkbox" />
        <span :class="{ done: todo.done }" class="todo-text">{{ todo.text }}</span>
        <button @click="removeTodo(todo)" class="remove-button">完了</button>
      </li>
    </ul>
    <button @click="hideCompleted = !hideCompleted" class="toggle-button">
      {{ hideCompleted ? '全タスク表示' : '未完了タスクを表示' }}
    </button>
  </div>
</template>

<style scoped>
.done {
  text-decoration: line-through;
  color: orange; /* 完了したタスクのテキスト色をオレンジに */
}

.checkbox {
  accent-color: orange; /* チェックボックスの色をオレンジに */
}

.todo-text {
  color: black; /* 未完了タスクのテキスト色を黒に設定 */
}

.remove-button {
  background-color: orange;
  border: 2px solid orange;
  color: white;
  padding: 3px 6px;
  border-radius: 4px;
  cursor: pointer;
}

.remove-button:hover {
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
  margin-top: 10px;
}

.toggle-button:hover {
  background-color: white;
  color: orange;
}

.todo-item {
  margin-bottom: 10px; /* タスク間のスペースを広げる */
}
</style>
