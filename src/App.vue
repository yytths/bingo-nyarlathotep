<template>
  <div id="app">
    <BingoBoard />
  </div>
</template>

<script>
import BingoBoard from './components/Bingo.vue'

export default {
  name: 'App',
  components: {
    BingoBoard
  }
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