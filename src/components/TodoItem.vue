<template>
  <li>
    <span :style="{ textDecoration: todo.isCompleted ? 'line-through' : 'none' }" v-if="!isEditing">{{ todo.text }}</span>
    <TodoEdit v-else :text="todo.text" @save="saveEdit" />

    <div class=button-div>
    <button @click="$emit('toggle-complete', index)">
    {{ todo.isCompleted ? "未完了" : "完了" }}
    </button>
    <button @click="toggleEdit">{{ isEditing ? "中止" : "編集" }}</button>
    <button @click="removeTodo">削除</button>
  </div>

<label for="priority-select">優先順位:</label>
    <select id="priority-select" v-model="localPriority" @change="updatePriority">
      <option value="1">低</option>
      <option value="2">中</option>
      <option value="3">高</option>
    </select>
  </li>
</template>

<script>
import TodoEdit from './TodoEdit.vue';
export default {
  name: 'TodoItem',
  components: {
    TodoEdit
  },
  props: {
    todo: {
      type: Object,
      required: true
    },
    index: {
    type: Number,
    required: true
  }
  },
  data() {
    return {
      isEditing: false,
      localPriority: this.todo.priority || 1
    };
  },
  methods: {
     toggleEdit() {
      this.isEditing = !this.isEditing;
    },
    saveEdit(newText) {
      this.$emit('edit', newText);
      this.isEditing = false;
    },
    removeTodo() {
      this.$emit('remove', this.todo);
    },
    updatePriority() {
      this.$emit('set-priority', this.index, this.localPriority);
    }
    
  }
};
</script>

<style scoped>
button {
  margin-left: 10px;
  padding: 4px 8px;
  background-color: #e74c3c;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.button-div{
  margin: 10px;
}

button:hover {
  background-color: #c0392b;
}
label{
  font-size: 12px;
    margin: 0 8px;
}
</style>


