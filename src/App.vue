<template>
  <div id="app">
    <h1 class="fw-bold">Sudoku</h1>
    <GameInfo 
      :difficulty="difficulty"
      :gameStopMessage="gameStopMessage"
      @game-start="onGameStart" 
      @game-stop="onGameStop"
      @game-submit="onGameSubmit"
      @change-difficulty="onChangeDifficulty"
    />
    <DifficultyInput />
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
import DifficultyInput from '@/components/DifficultyInput'
import Board from '@/components/Board.vue'
import NumberInput from '@/components/NumberInput.vue'

// initialize ansSeed -> add more seeds later?
const ansSeed = [
  [4, 2, 6, 5, 7, 1, 3, 9, 8],
  [8, 5, 7, 2, 9, 3, 1, 4, 6],
  [1, 3, 9, 4, 6, 8, 2, 7, 5],
  [9, 7, 1, 3, 8, 5, 6, 2, 4],
  [5, 4, 3, 7, 2, 6, 8, 1, 9],
  [6, 8, 2, 1, 4, 9, 7, 5, 3],
  [7, 9, 4, 6, 3, 2, 5, 8, 1],
  [2, 6, 5, 8, 1, 4, 9, 3, 7],
  [3, 1, 8, 9, 5, 7, 4, 6, 2]
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
      'selected': false,
      'answered': false,
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
    DifficultyInput,
    Board,
    NumberInput
  },

  data: function () {
    return {
      // for Board
      ansBoard: _.cloneDeep(ansSeed),
      board: _.cloneDeep(board),
      squareClasses: _.cloneDeep(squareClasses),
      selectedSquare: 0,
      difficulty: 'Easy',
      squaresToReveal: [],
      gameStopMessage: ''
    }
  },

  methods: {
    // Events
    onGameStart: function () {
      console.log('Game Start')

      // reset all
      this.board = _.cloneDeep(board)
      this.squareClasses = _.cloneDeep(squareClasses)
      this.gameStopMessage = ''

      // make ansBoard from ansSeed
      this.makeNewAnsBoard()

      // choose fixed squares according to difficulty
      this.chooseSquaresToReveal()

      // reveal fixed squares on the board
      this.revealChosenSquares()
    },
    onGameStop: function () {
      this.board = board
      this.gameStopMessage = ''
      this.onSquareClick([0, 0])
    },
    onGameSubmit: function (playTime) {
      let wrongSubmit = false
      for (let i=0; i<9; i++) {
        for (let j=0; j<9; j++) {
          if (this.ansBoard[i][j] != this.board[i][j]) {
            wrongSubmit = true
            break
          }
        }
        if (wrongSubmit) {
          this.gameStopMessage = 'Wrong Answer'
          break
        }
      }
      if (!wrongSubmit) {
        this.gameStopMessage = `Congratulations! You completed ${this.difficulty} in ${playTime}`
        // this.setRecord(playTime)
      }
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

      // check if fixed square
      if (!this.squaresToReveal.includes(9*r + c)) {
        this.board = _.cloneDeep(this.board)
        this.board[r][c] = num
        this.squareClasses[9*r + c].answered = true
      }
    },
    onChangeDifficulty: function () {
      if (this.difficulty == 'Easy') {
        this.difficulty = 'Medium'
      } else if (this.difficulty == 'Medium') {
        this.difficulty = 'Hard'
      } else {
        this.difficulty = 'Easy'
      }
    },

    // Game Initialization
    makeNewAnsBoard: function () {
      // make new board from seed
      console.log('## initial board')
      console.log(this.ansBoard)

      // make 20 changes to seed answer board
      for (let cnt=0; cnt<20; cnt++) {
        const change = _.sample([1, 2, 3, 4])
        switch (change) {
          case 1:
            this.transpose()
            break
          case 2:
            this.swapNumPos()
            break
          case 3:
            this.rotateClockwise()
            break
          case 4:
            this.switchRowsInBlock()
            break
        }
      }
      console.log('## changed board')
      console.log(this.ansBoard)
    },
    chooseSquaresToReveal: function () {
      // Easy: 35, Medium: 25, Hard: 20
      const candidateSquares = Array.from(Array(81), (e, i) => i)
      let numOfSquaresToReveal

      if (this.difficulty === 'Easy') {
        numOfSquaresToReveal = 35
      } else if (this.difficulty === 'Medium') {
        numOfSquaresToReveal = 27
      } else {
        numOfSquaresToReveal = 20
      }

      const squaresToReveal = _.sampleSize(candidateSquares, numOfSquaresToReveal)
      this.squaresToReveal = squaresToReveal
    },
    revealChosenSquares: function () {
      const newBoard = _.cloneDeep(this.board)

      for (let i=0; i<9; i++) {
        for (let j=0; j<9; j++) {
          if (this.squaresToReveal.includes(9*i + j)) {
            newBoard[i][j] = this.ansBoard[i][j]
          }
        }
      }
      this.board = newBoard
    },

    // Matrix transformations
    transpose: function () {
      console.log('transpose matrix')
      this.ansBoard = this.ansBoard.reduce(($, row) => row.map((_, i) => [...($[i] || []), row[i]]), [])
    },
    swapNumPos: function () {
      // choose 2 nums from 1 to 9
      const [a, b] = _.sampleSize(Array.from(Array(9), (e, i)=>i+1), 2)

      // create new matrix with a, b switched positions
      const newMatrix = _.cloneDeep(this.ansBoard)

      for (let i=0; i<9; i++) {
        for (let j=0; j<9; j++) {
          if (this.ansBoard[i][j] === a) {
            newMatrix[i][j] = b
          }
          if (this.ansBoard[i][j] === b) {
            newMatrix[i][j] = a
          }
        }
      }
      console.log(`swap positions of ${a} and ${b}`)
      this.ansBoard = newMatrix
    },
    rotateClockwise: function () {
      const newMatrix = []
      for(let i = 0; i < this.ansBoard[0].length; i++) {
          let row = this.ansBoard.map(e => e[i]).reverse()
          newMatrix.push(row)
      }
      console.log('rotateClockwise')
      this.ansBoard = newMatrix
    },
    switchRowsInBlock: function () {
      const block = _.sample([0, 3, 6])
      const [a, b] = _.sampleSize([block, block+1, block+2], 2)
      console.log(`change rows ${a} and ${b}`)

      const rowA = this.ansBoard[a]
      const rowB = this.ansBoard[b]

      const newMatrix = _.cloneDeep(this.ansBoard)

      newMatrix[a] = rowB
      newMatrix[b] = rowA
      this.ansBoard = newMatrix
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
