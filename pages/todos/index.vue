<template>
   <div style="text-align: center;">
    <input v-model="todoText" type="text" />
    <button @click="addTodo" class="d-flex justify-content-between mt-5 flex-nowrap flex-column">Add</button>
  </div>
    <div class="card">
      <ul>
        <li v-for="todo in todos" :key="todo.id">
          <div >
            <div>
              <span>ID: {{ todo.id }}</span>
              <span>Name: {{ todo.name }}</span>
            </div>
            <div class="card">
              <label>Status:</label>
              <select v-model="todo.status">
                <option>Pending</option>
                <option>Doing</option>
                <option>Done</option>
              </select>
              <button @click="editTodo(todo.id, todo)" class="mr-4 px-4 py-2 bg-blue-500 text-white rounded">Update</button>
              <nuxt-link :to="`/todos/${todo.id}`" class="px-4 py-2 bg-green-500 text-white rounded">Edit</nuxt-link>

            </div>
          </div>
        </li>
      </ul>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  const BASE_URL = "https://655eb283879575426b43d0a4.mockapi.io";
  
  const todos = ref([]);
  const todoText = ref("");
  
  const loadTodo = async () => {
    const response = await fetch(`${BASE_URL}/todos`);
    const data = await response.json();
    todos.value = data;
  };
  
  const addTodo = async () => {
    try {
      await fetch(`${BASE_URL}/todos`, {
        method: "post",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          name: todoText.value,
          status: "Pending",
        }),
      });
      await loadTodo();
    } catch (error) {
      console.log('error', error);
    }
  };
  
  const editTodo = async (todoId, todoData) => {
    try {
      await fetch(`${BASE_URL}/todos/${todoId}`, {
        method: "put",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          status: todoData.status,
        }),
      });
      await loadTodo();
    } catch (error) {
      console.log('error', error);
    }
  };
  loadTodo();
  </script>
  
  <style scoped>
  .card {
    border: 1px solid #0000cc;
    margin: 50px;
    padding: 20px;
    font-size: 16px;
    text-align: center;
  }
  </style>
  