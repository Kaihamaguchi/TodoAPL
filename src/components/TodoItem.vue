<script setup>
import { ref, computed, defineProps, defineEmits } from "vue";

const props = defineProps({
  todos: {
    type: Array,
    required: true
  }
});

const emit = defineEmits(['update:todos']);

const hideCompleted = ref(false);

const filteredTodos = computed(() => {
  return hideCompleted.value ? props.todos.filter(t => !t.done) : props.todos;
});

function removeTodo(todo) {
  const updatedTodos = props.todos.filter(t => t !== todo);
  emit('update:todos', updatedTodos); // 親コンポーネントに削除後の配列を通知
}
</script>

<template>
  <div>
    <p v-if="todos.length === 0">ToDoがまだありません！</p>
    <ul>
      <li v-for="todo in filteredTodos" :key="todo.id" class="todo-item">
        <input type="checkbox" v-model="todo.done" class="checkbox" />
        <span :class="{ done: todo.done }" class="todo-text">{{ todo.title }}</span>
        <button @click="removeTodo(todo)" class="remove-button">削除</button>
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
  color: orange;
}

.checkbox {
  accent-color: orange;
}

.todo-text {
  color: black;
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
  margin-bottom: 10px;
}
</style>
