<template>
  <div class="d-flex justify-content-center">
    <div 
      class="number-btn" 
      :class="numberClasses[num]"
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

  props: {
    numberClasses: Object
  },

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
}
</style>