<script setup>
import { ref, computed, defineProps, defineEmits } from "vue";
//ref: Vue.js のリアクティブな変数を作成するために使用されます。これにより、変数の値が変更されると自動的に UI が更新されます。
//computed: 他の値に依存する計算されたプロパティを定義するために使用されます。依存する値が変わると、自動的に再計算されます。
//defineProps: 親コンポーネントから渡されたプロパティ (props) を定義します。
//defineEmits: 親コンポーネントにイベントを通知するための仕組み (emit) を定義します。

const props = defineProps({
  todos: {
    type: Array,
    required: true,
    //props: 親コンポーネントから todos という配列が渡されます。この配列には、すべての ToDo 項目が格納されています。
    //emit: 親コンポーネントに update:todos というイベントを発火させるための関数を定義します。これにより、子コンポーネントが親コンポーネントに対して変更を通知できます。
  },
});

const emit = defineEmits(["update:todos"]);

const hideCompleted = ref(false);
//hideCompleted: 完了したタスクを表示するかどうかを制御するためのブール値です。デフォルトでは false で、すべてのタスクが表示されます。

const filteredTodos = computed(() => {
  //filteredTodos: タスクのリストをフィルタリングする計算プロパティです。hideCompleted が true の場合、未完了のタスクのみを表示します。それ以外の場合、すべてのタスクを表示します。
  return hideCompleted.value ? props.todos.filter((t) => !t.done) : props.todos;
});

function removeTodo(todo) {
  const updatedTodos = props.todos.filter((t) => t !== todo);
  emit("update:todos", updatedTodos); // 親コンポーネントに削除後の配列を通知
}
function formatDate(date) {
  return new Date(date).toLocaleDateString('ja-JP'); // 日本の日付形式に変換
}
</script>

<template>
  <div>
    <p v-if="todos.length === 0">ToDoがまだありません！</p>
    <ul>
      <li v-for="todo in filteredTodos" :key="todo.id" class="todo-item">
        <input type="checkbox" v-model="todo.done" class="checkbox" />
        <span :class="{ done: todo.done }" class="todo-text">{{
          todo.title
        }}</span>
        <span v-if="todo.deadline" class="deadline"
          >   期限: {{ formatDate(todo.deadline) }}</span
        >
        <span v-else class="no-deadline">   期限なし</span>
        <button @click="removeTodo(todo)" class="remove-button">削除</button>
      </li>
    </ul>
    <button @click="hideCompleted = !hideCompleted" class="toggle-button">
      {{ hideCompleted ? "全タスク表示" : "未完了タスクを表示" }}
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
