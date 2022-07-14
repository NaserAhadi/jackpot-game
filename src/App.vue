<template>
  <div id="app">
    <GameTable />
  </div>
</template>

<script>
import GameTable from './components/GameTable.vue'


export default {
  name: 'App',
  components: {
    GameTable
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
