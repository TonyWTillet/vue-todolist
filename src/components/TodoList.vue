<template>

  <Layout>
    
    <template #main>
      <div class="todo-container">
        <h2>My Todo List</h2>
        <p v-if="remainingTodos > 0">Remaining todos: {{ remainingTodos }}</p>
        <form class="todo-form" @submit.prevent="addTodo">
          <input type="text" v-model="newTodo.title" placeholder="Title">
          <input type="text" v-model="newTodo.description" placeholder="Description">
          <button :disabled="newTodo.title === ''" class="btn btn-primary">Add Todo</button>
        </form>

        <div class="todo-grid">
          <p v-if="todos.length === 0">No todos yet</p>
          <div v-else class="todo-card" v-for="todo in sortedTodos" :key="todo.id" :class="todo.color">
            <div class="todo-card-content">
              <div>
                <input type="checkbox" v-model="todo.isDone">
                <span :class="todo.isDone ? 'todo-card-status-done' : 'todo-card-status-pending'" class="todo-card-status" style="margin-left: 10px;">{{ todo.isDone ? 'Done' : 'Pending' }}</span>
              </div>
              <div class="todo-card-header"> 
                <h3 :class="todo.isDone ? 'todo-title-done' : 'todo-title'" class="todo-title">{{ todo.title }}</h3>
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

        <div>
          <Checkbox v-model="hideDone" :label="'Hide done todos'" @check="(p) => console.log('checked', p)" @uncheck="(p) => console.log('uncheck', p)" />
        </div>
      </div> 
    </template>
  </Layout>
  

  
</template>


<script setup>
import { computed, ref } from 'vue';  
import Checkbox from './inputs/Checkbox.vue';
import Layout from './Layouts.vue';

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
  };
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

const hideDone = ref(false);

const sortedTodos = computed(() => {
  const sortedTodos = todos.value.toSorted((a, b) => 
     a.isDone > b.isDone ? 1 : -1
  );

  if (hideDone.value === true) {
    return sortedTodos.filter(todo => !todo.isDone);
  }

  return sortedTodos;
});

const remainingTodos = computed(() => {
  return todos.value.filter(todo => !todo.isDone).length;
});

</script>