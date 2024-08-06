<script setup>
import { ref, computed, defineProps } from "vue";

// `props` で受け取る
const props = defineProps({
  todos: {
    type: Array,
    required: true
  }
});

const hideCompleted = ref(false);

const filteredTodos = computed(() => {
  return hideCompleted.value ? props.todos.filter((t) => !t.done) : props.todos;
});

function removeTodo(todo) {
  // `props.todos` を更新する形にする
  props.todos.splice(props.todos.indexOf(todo), 1);
}
</script>

<template>
  <div>
    <p v-if="props.todos.length === 0">ToDoがまだありません！</p>
    <ul>
      <li v-for="todo in filteredTodos" :key="todo.id">
        <input type="checkbox" v-model="todo.done" />
        <span :class="{ done: todo.done }">{{ todo.text }}</span>
        <button @click="removeTodo(todo)">完了</button>
      </li>
    </ul>
    <button @click="hideCompleted = !hideCompleted">
      {{ hideCompleted ? '全タスク表示' : '未完了タスクを表示' }}
    </button>
  </div>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>
