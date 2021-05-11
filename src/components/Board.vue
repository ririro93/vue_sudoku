<template>
<div class="my-3">
  <div class="d-flex justify-content-center" v-for="(row, r) in board" :key="r" >
    <div 
      class="square" 
      v-bind:class="{ 'border-right': borderRight[r][c], 'border-bot': borderBottom[r][c] }" 
      v-for="(square, c) in row" 
      :key="9*r + c"
      @click="onClickSquare(r, c)"
    >
      {{ board[r][c] }}
    </div>
  </div>
</div>
</template>

<script>
const board = []
const borderRight = []
const borderBottom = []

for (let i=0; i<9; i++) {
  let rows = []
  let rowRight = []
  let rowBottom = []
  for (let j=0; j<9; j++) {
    rows.push('')
    if (j === 2 || j === 5 ) {
      rowRight.push(true)
    } else {
      rowRight.push(false)
    }
    if (i === 2 || i === 5) {
      rowBottom.push(true)
    } else {
      rowBottom.push(false)
    }
  }
  board.push(rows)
  borderRight.push(rowRight)
  borderBottom.push(rowBottom)
}

export default {
  name: 'Board',

  data: function () {
    return {
      board,
      borderRight,
      borderBottom
    }
  },

  methods: {
    onClickSquare: function (r, c) {
      console.log(r, c, 'value:', this.board[r][c])
    }
  }
}
</script>

<style>
.square {
  width: 5vh;
  height: 5vh;
  line-height: 5vh;
  border: 1px solid lightgray;
}

.border-right {
  border-right: 2px black solid;
}

.border-bot{
  border-bottom: 2px black solid;
}
</style>