<template>
<div class="my-3">
  <div class="d-flex justify-content-center" v-for="(row, r) in board" :key="r" >
    <div 
      class="square" 
      :class="squareClasses[9*r + c]" 
      v-for="(square, c) in row" 
      :key="9*r + c"
      @click="onClickSquare(r, c)"
      v-text="board[r][c]"
    >
    </div>
  </div>
</div>
</template>

<script>


export default {
  name: 'Board',

  props: {
    board: Array,
    squareClasses: Object
  },

  methods: {
    onClickSquare: function (r, c) {
      this.$emit('square-click', [r, c])
    }
  }
}
</script>

<style>
.square {
  width: 5vh;
  height: 5vh;
  line-height: 5vh;
  font-size: 3vh;
  border: 1px solid lightgray;
}

.border-right {
  border-right: 2px black solid;
}

.border-bot {
  border-bottom: 2px black solid;
}

.selected {
  background-color: lightgray;
}
</style>