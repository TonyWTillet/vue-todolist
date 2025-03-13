<template>

  <div class="todo-container">
    <h2>My Todo List</h2>

    <form class="todo-form" @submit.prevent="addTodo">
      <input type="text" v-model="newTodo.title" placeholder="Title">
      <input type="text" v-model="newTodo.description" placeholder="Description">
      <button class="btn btn-primary">Add Todo</button>
    </form>

    <div class="todo-grid">
      <p v-if="todos.length === 0">No todos yet</p>
      <div v-else class="todo-card" v-for="todo in todos" :key="todo.id">
        <div class="todo-card-content" :class="todo.color">
          <span :class="todo.isDone ? 'todo-card-status-done' : 'todo-card-status-pending'" class="todo-card-status">{{ todo.isDone ? 'Done' : 'Pending' }}</span>
          <div class="todo-card-header"> 
            <h3 class="todo-title">{{ todo.title }}</h3>
            <p class="todo-date">{{ todo.date }}</p>
          </div>
          <div class="todo-card-body">
            <p>{{ todo.description }}</p>
          </div>
          <div class="todo-card-footer" v-if="!todo.isDone">
            <button @click="markAsDone(todo.id)" class="btn btn-success mt-2" data-id="todo.id">Mark as done</button>
          </div>
        </div>
      </div>
    </div>
  </div>

  
</template>

<script setup>
import { ref } from 'vue';  

const todos = ref([]);
const colors = ['todo-blue', 'todo-purple', 'todo-green', 'todo-orange'];
const randomColor = () => {
  return colors[Math.floor(Math.random() * colors.length)];
}


const newTodo = ref({
  id: todos.value.length + 1,
  color: randomColor(),
  title: '',
  description: '',
  isDone: false,
  date: new Date().toLocaleDateString(),
});

const addTodo = () => {
  todos.value.push(newTodo.value);
  newTodo.value = {
    id: todos.value.length + 1,
    color: randomColor(),
    title: '',
    description: '',
    isDone: false,
    date: new Date().toLocaleDateString(),
  };s
};

const markAsDone = (id) => {
  todos.value.find(todo => todo.id === id).isDone = true;

  // Display a success message with a fade out effect
  const successMessage = document.createElement('div');
  successMessage.classList.add('success-message');
  successMessage.textContent = 'Todo marked as done';
  document.body.appendChild(successMessage);
  setTimeout(() => {
    successMessage.style.opacity = '0';
    setTimeout(() => {
      successMessage.remove();
    }, 300);
  }, 3000);
}
</script>