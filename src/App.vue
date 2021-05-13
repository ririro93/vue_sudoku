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

// initializa andSeed
const ansSeed = [
  [4, 2, 6, 5, 7, 1, 3, 9, 8],
  [8, 5, 7, 2, 9, 3, 1, 4, 6],
  [1, 3, 9, 4, 6, 8, 2, 7, 5],
  [9, 7, 1, 3, 8, 5, 6, 2, 4],
  [5, 4, 3, 7, 2, 6, 8, 1, 9],
  [6, 8, 2, 1, 4, 9, 7, 5, 3],
  [7, 9, 4, 6, 3, 2, 5, 8, 1],
  [2, 6, 5, 8, 1, 4, 9, 3, 7],
  [3, 1, 8, 9, 5, 7, 4, 6, 5]
]

// initialize board
const board = []
const squareClasses = {}
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
      ansSeed,
      board,
      squareClasses,
      selectedSquare: 0,
    }
  },

  methods: {
    onGameStart: function () {
      console.log('Game Start')

      // make ansBoard from ansSeed
      this.makeNewAnsBoard()
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
    },

    makeNewAnsBoard: function () {
      // make new board from seed
      console.log(ansSeed)
      console.log('transposed', this.transpose(ansSeed))
      console.log('switched', this.switchNumPos(ansSeed))

      // choose # of shown squares taking into account difficulty

      // choose shown squares
    },

    transpose: function (matrix) {
      return matrix.reduce(($, row) => row.map((_, i) => [...($[i] || []), row[i]]), [])
    },

    switchNumPos: function (matrix) {
      // choose 2 nums from 1 to 9
      const [a, b] = _.sampleSize(Array.from(Array(9), (e, i)=>i+1), 2)

      // create new matrix with a, b switched positions
      const newMatrix = _.cloneDeep(matrix)

      for (let i=0; i<9; i++) {
        for (let j=0; j<9; j++) {
          if (matrix[i][j] === a) {
            newMatrix[i][j] = b
          }
          if (matrix[i][j] === b) {
            newMatrix[i][j] = a
          }
        }
      }
      console.log(a, b)
      return newMatrix
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
