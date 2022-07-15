<template>
  <div id="app">
    <h1 class="title">Welcome to jackpot game!</h1>
    <GameTable />
    <notifications position="top center" width="40%" :max="3"/>
  </div>
</template>

<script>
import GameTable from './components/GameTable/GameTable.vue'


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

.title{
  width: 80%;
  margin: auto;
  color: #40C1B6;
  text-align: left;
}
</style>
