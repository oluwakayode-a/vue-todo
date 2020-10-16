<template>
  <div id="app">
    <h1 class="title">My Todo</h1>
    <Todos />    
  </div>
</template>

<script>
import Todos from "./components/Todos.vue";

export default {
  name: 'App',
  components: {
    Todos
  }
}
</script>

<style>
#app {
  font-family: 'Poppins';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.title {
  font-size: 2rem;
  color: #707070;
}
</style>
