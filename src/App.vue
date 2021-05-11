<template>
  <div id="app">
    <h1>Sudoku</h1>
    <GameInfo @game-start="onGameStart" />
    <Board 
      :board="board"
      :squareClasses="squareClasses"
      @square-click="onSquareClick"
    />
    <NumberInput 
      :numberClasses="numberClasses"
      @number-click="onNumberClick"
    />
  </div>
</template>

<script>
import GameInfo from '@/components/GameInfo.vue'
import Board from '@/components/Board.vue'
import NumberInput from '@/components/NumberInput.vue'

const board = []
const squareClasses = {}
const numberClasses = {}

// initialize Board
for (let i=0; i<9; i++) {
  let rows = []
  for (let j=0; j<9; j++) {
    const squareId = 9 * i + j
    squareClasses[squareId] = {
      'border-right': false,
      'border-bot': false,
      'selected': false
    }
    rows.push('')
    if (j === 2 || j === 5 ) {
      squareClasses[squareId]['border-right'] = true
    }
    if (i === 2 || i === 5) {
      squareClasses[squareId]['border-bot'] = true
    }
  }
  board.push(rows)
}

// initialize NumberInput
for (let i=1; i<10; i++) {
  numberClasses[i] = {'selected': false}
}

export default {
  name: 'App',

  components: {
    GameInfo,
    Board,
    NumberInput
  },

  data: function () {
    return {
      // for Board
      board,
      squareClasses,
      selectedSquare: 0,

      // for NumberInput
      numberClasses,
      selectedNumber: 1
    }
  },

  methods: {
    onGameStart: function () {
      console.log('Game Start from app')
    },
    onSquareClick: function ([r, c]) {
      this.squareClasses[this.selectedSquare]['selected'] = false
      this.selectedSquare = 9 * r + c
      this.squareClasses[this.selectedSquare]['selected'] = true
    },
    onNumberClick: function (num) {
      this.numberClasses[this.selectedNumber]['selected'] = false
      this.selectedNumber = num
      this.numberClasses[this.selectedNumber]['selected'] = true
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
