<template>
  <div id="app">
    <memory-game :columns="4"/>
  </div>
</template>

<script>
import memoryGame from './components/MemoryGame.vue'

export default {
  name: 'App',
  components: {
    memoryGame
   },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
