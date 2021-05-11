<template>
  <div class="d-flex justify-content-center">
    <div 
      class="btn btn-success number-btn" 
      v-for="num in numbers" 
      :key="num"
      @click="onNumberClick(num)"
    >
      {{ num }}
    </div>
  </div>
</template>

<script>
const numbers = []
for (let i=1; i < 10; i++) {
  numbers.push(i)
}

export default {
  name: 'NumberInput',

  data: function () {
    return {
      numbers
    }
  },

  methods: {
    onNumberClick: function (num) {
      this.$emit('number-click', num)
    }
  }


}
</script>

<style>
.number-btn {
  border: 1px solid black;
  width: 7vh;
  height: 7vh;
  line-height: 7vh;
  padding: 0;
  font-size: 3vh;
}
</style>