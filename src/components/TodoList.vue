<template>
<div class="todo-list">
<h1>To-Do List</h1>
<input v-model="newTodo" @keyup.enter="addTodo" placeholder="入力してエンターを押してください。" />

<div class="filters">
      <button @click="filter = 'all'" :class="{ active: filter === 'all' }">全て</button>
      <button @click="filter = 'completed'" :class="{ active: filter === 'completed' }">完了</button>
      <button @click="filter = 'incomplete'" :class="{ active: filter === 'incomplete' }">未完了</button>
    </div>
<ul>
      <TodoItem
        v-for="(todo, index) in  filteredTodos"
        :key="index"
        :todo="todo"
        :index="index"
        @remove="removeTodo"
        @edit="editTodo(todo, $event)"
        @toggle-complete="toggleComplete(todo)" 
        @set-priority="updatePriority"
      />
    </ul>
  </div>
</template>

<script>
import TodoItem from './TodoItem.vue';

export default {
  name: 'TodoList',
  components: {
    TodoItem
  },
  data() {
    return {
      newTodo: '',
      todos: [],
      filter: 'all'
    };
  },

  mounted() {
  const savedTodos = localStorage.getItem('todos');
  if (savedTodos) {
    this.todos = JSON.parse(savedTodos);
  }
},

watch: {
  todos: {
    handler(newTodos) {
      localStorage.setItem('todos', JSON.stringify(newTodos));
    },
    deep: true  
  }
},

computed: {
    filteredTodos() {
      if (this.filter === 'completed') {
        return this.todos.filter(todo => todo.isCompleted);
      } else if (this.filter === 'incomplete') {
        return this.todos.filter(todo => !todo.isCompleted);
      }
      return this.todos;
    }
  },


  methods: {
    addTodo() {
      if (this.newTodo.trim() !== '') {
        this.todos.push({ text: this.newTodo, isCompleted: false,priority: "1"  });
        this.newTodo = '';
      }
    },
    removeTodo(todoToDelete) {
      const index = this.todos.indexOf(todoToDelete);
      if (index !== -1) {
        this.todos.splice(index, 1);
      }
    },

    editTodo(todo, newText) {
      todo.text = newText;
    },

    toggleComplete(todo) {
      todo.isCompleted = !todo.isCompleted;
    },

    updatePriority(index, newPriority) {
      const todo = this.todos[index];
      if (todo) {
        todo.priority = newPriority;
      }
    }
    
  }
};
</script>

<style scoped>
.todo-list {
  max-width: 400px;
  margin: 0 auto;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: #f9f9f9;
}

@media screen and (min-width:768px) {
  
.todo-list {
  max-width: 500px;
}
}

input {
  width: 90%;
  padding: 8px;
  margin-bottom: 20px;
}

button{
  background: orange;
    border-color: orange;
    border-radius: 4px;
    color: white;
    margin-right: 5px;
    
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  padding: 8px;
  border-bottom: 1px solid #ddd;
}

li:last-child {
  border-bottom: none;
}
</style>