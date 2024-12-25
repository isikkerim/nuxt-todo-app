<template>
    <div class="container mx-auto p-4">
      <h1 class="text-3xl font-bold mb-6 text-center">Nuxt Todo Uygulaması</h1>
  
      <div class="max-w-md mx-auto bg-white rounded-lg shadow-md p-6">
        <div class="mb-4 flex">
          <input type="text" v-model="newTodoText" placeholder="Yeni todo ekle"
                 class="border border-gray-300 rounded-md px-4 py-2 w-full focus:outline-none focus:ring-2 focus:ring-blue-500">
          <button @click="addTodo"
                  class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-md ml-2">
            Ekle
          </button>
        </div>
  
        <ul class="space-y-2">
          <li v-for="todo in todos" :key="todo.id"
              class="bg-gray-50 rounded-md p-3 flex items-center justify-between hover:bg-gray-100 transition">
            <div class="flex items-center">
              <input type="checkbox" v-model="todo.completed" @change="saveTodos" class="mr-2 h-5 w-5 rounded border border-gray-300 focus:ring-2 focus:ring-blue-500">
              <span :class="{ 'line-through': todo.completed }" class="flex-grow">{{ todo.text }}</span>
            </div>
  
            <div class="flex">
              <button @click="editTodo(todo)" class="bg-yellow-500 hover:bg-yellow-700 text-white font-bold py-1 px-2 rounded-md mr-1">Düzenle</button>
              <button @click="removeTodo(todo.id)" class="bg-red-500 hover:bg-red-700 text-white font-bold py-1 px-2 rounded-md">Sil</button>
            </div>
          </li>
        </ul>
  
        <div v-if="editingTodo" class="mt-4">
          <input type="text" v-model="editingTodo.text"
                 class="border border-gray-300 rounded-md px-4 py-2 w-full focus:outline-none focus:ring-2 focus:ring-blue-500">
          <div class="mt-2 flex justify-end">
            <button @click="saveEdit" class="bg-green-500 hover:bg-green-700 text-white font-bold py-1 px-2 rounded-md mr-1">Kaydet</button>
            <button @click="cancelEdit" class="bg-gray-500 hover:bg-gray-700 text-white font-bold py-1 px-2 rounded-md">İptal</button>
          </div>
        </div>
      </div>
    </div>
  </template>
  

  
  <script setup lang="ts">
  import { ref, onMounted, watch } from 'vue';
  import { v4 as uuidv4 } from 'uuid';
  
  interface Todo {
    id: string;
    text: string;
    completed: boolean;
  }
  
  const todos = ref<Todo[]>([]);
  const newTodoText = ref('');
  const editingTodo = ref<Todo | null>(null);
  
  const loadTodos = () => {
    const storedTodos = localStorage.getItem('todos');
    if (storedTodos) {
      todos.value = JSON.parse(storedTodos);
    }
  };
  
  const saveTodos = () => {
    localStorage.setItem('todos', JSON.stringify(todos.value));
  };
  
  const addTodo = () => {
    if (newTodoText.value.trim() !== '') {
      const newTodo: Todo = { id: uuidv4(), text: newTodoText.value, completed: false };
      todos.value.push(newTodo);
      newTodoText.value = '';
      saveTodos();
    }
  };
  
  const removeTodo = (id: string) => {
    todos.value = todos.value.filter(todo => todo.id !== id);
    saveTodos();
  };
  
  const editTodo = (todo: Todo) => {
    editingTodo.value = { ...todo };
  };
  
  const saveEdit = () => {
    if (editingTodo.value) {
      const index = todos.value.findIndex(t => t.id === editingTodo.value!.id);
      if (index !== -1) {
        todos.value.splice(index, 1, editingTodo.value);
        saveTodos();
      }
      editingTodo.value = null;
    }
  };
  
  const cancelEdit = () => {
    editingTodo.value = null;
  };
  
  onMounted(() => {
    loadTodos();
  });
  
  watch(todos, () => {
    saveTodos();
  }, { deep: true });
  </script>