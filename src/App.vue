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
      @number-click="onNumberClick"
    />
  </div>
</template>

<script>
import _ from 'lodash'
import GameInfo from '@/components/GameInfo.vue'
import Board from '@/components/Board.vue'
import NumberInput from '@/components/NumberInput.vue'

const board = []
const squareClasses = {}

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
    if (i === 0 && j === 0) {
      squareClasses[squareId]['selected'] = true
    }
    if (j === 2 || j === 5 ) {
      squareClasses[squareId]['border-right'] = true
    }
    if (i === 2 || i === 5) {
      squareClasses[squareId]['border-bot'] = true
    }
  }
  board.push(rows)
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
    }
  },

  methods: {
    onGameStart: function () {
      console.log('Game Start from app')
    },

    onSquareClick: function ([r, c]) {
      // change selectedSquare
      this.squareClasses[this.selectedSquare]['selected'] = false
      this.selectedSquare = 9 * r + c
      this.squareClasses[this.selectedSquare]['selected'] = true
    },

    onNumberClick: function (num) {
      // change selectedSquare to selectedNumber
      const r = Math.floor(this.selectedSquare / 9)
      const c = this.selectedSquare % 9
      this.board = _.cloneDeep(this.board)
      this.board[r][c] = num
    }
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
