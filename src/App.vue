<script setup>
import TodoList from './components/TodoList.vue';
</script>

<template>
  <main>
    <div>
      <h1>Welcome to the Todo List App</h1>
    </div>

    <TodoList />
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}


@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
