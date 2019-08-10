<template>
  <div id="app">
    <TodoApp id="todoapp"/>
  </div>
</template>

<script>
import TodoApp from './components/TodoApp.vue'

export default {
  name: 'app',
  components: {
    TodoApp
  }
}
</script>

<style>
#app {
  font-family: "Helvetica Neue",Helvetica,Arial,sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
#todoapp {
  max-width: 400px;
  margin:0 auto;
}
</style>
