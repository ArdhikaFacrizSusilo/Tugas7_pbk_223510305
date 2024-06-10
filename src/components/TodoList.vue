<template>
    <div class="todo-list">
      <h1>Daftar Tugas</h1>
      <div class="input-container">
        <input v-model="newTodo" placeholder="Tambahkan tugas baru" />
        <button @click="addTodo">Tambah</button>
      </div>
      <ul>
        <li v-for="(todo, index) in todos" :key="index">
          <label>
            <input type="checkbox" v-model="todo.completed" />
            <span :class="{ completed: todo.completed }">{{ todo.text }}</span>
          </label>
          <button @click="removeTodo(index)">Hapus</button>
        </li>
      </ul>
      <p>Total incomplete tasks: {{ incompleteTodosCount }}</p>
    </div>
  </template>
  
  <script>
  import { ref, computed } from 'vue'
  import { useTodoStore } from '../store/todo'
  
  export default {
    setup() {
      const store = useTodoStore()
      const newTodo = ref('')
  
      const addTodo = () => {
        if (newTodo.value.trim()) {
          store.addTodo(newTodo.value.trim())
          newTodo.value = ''
        }
      }
  
      return {
        newTodo,
        addTodo,
        todos: computed(() => store.todos),
        removeTodo: store.removeTodo,
        incompleteTodosCount: computed(() => store.incompleteTodosCount)
      }
    }
  }
  </script>
  
  <style scoped>
  .todo-list {
    max-width: 800px;
    margin: 40px auto;
    padding: 50px;
    background-color: #242424;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    border-radius: 8px;
    overflow-x: auto;
}
  
  .input-container {
    display: flex;
    gap: 10px;
    margin-bottom: 10px;
  }
  
  input[type="text"] {
    flex: 1;
    padding: 10px;
  }
  
  button {
    background-color: #4caf50;
    color: white;
    border: none;
    padding: 10px;
    cursor: pointer;
  }
  
  button:hover {
    background-color: #45a049;
  }
  
  .completed {
    text-decoration: line-through;
  }
  
  .todo-list ul {
    list-style-type: none;
    padding: 0;
  }
  
  .todo-list li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 10px;
  }
  
  .todo-list li label {
    display: flex;
    align-items: center;
  }
  
  .todo-list li label input[type="checkbox"] {
    margin-right: 10px;
  }
  
  .todo-list li button {
    background-color: #ff6b6b;
    color: white;
    border: none;
    padding: 5px 10px;
    cursor: pointer;
  }
  
  .todo-list li button:hover {
    background-color: #ff4c4c;
  }
  </style>
