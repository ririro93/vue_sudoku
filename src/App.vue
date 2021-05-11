<template>
  <div id="app">
    <h1>Sudoku</h1>
    <GameInfo @game-start="onGameStart" />
    <Board />
  </div>
</template>

<script>
import GameInfo from '@/components/GameInfo.vue'
import Board from '@/components/Board.vue'
// import NumberInput from '@/components/NumberInput.vue'

export default {
  name: 'App',

  components: {
    GameInfo,
    Board,
    // NumberInput
  },

  methods: {
    onGameStart: function () {
      console.log('Game Start from app')
    }
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
