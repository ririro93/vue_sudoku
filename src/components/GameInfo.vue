<template>
  <div>
    <div v-text="playTime"></div>
    <button class="btn btn-primary" @click="startGame">{{ gameButton }}</button>
    <button class="btn btn-warning" @click="stopGame" v-if="interval">Stop Game</button>
  </div>
</template>

<script>
export default {
  name: 'GameInfo',
  
  data: function () {
    return {
      startTime: 0,
      playTime: '00:00',
      interval: null
    }
  },

  methods: {
    startGame: function () {
      clearInterval(this.interval)
      this.startTime = new Date().getTime();
      this.interval = setInterval(() => {
        const diff = new Date().getTime() - this.startTime
        const mins = Math.floor((diff / 60000) % 60).toString().padStart(2, '0')
        const secs = Math.floor((diff / 1000) % 60).toString().padStart(2, '0')
        this.playTime = `${mins}:${secs}`
      }, 1000)

      // Tell parent about new game
      this.$emit('game-start')
    },

    stopGame: function () {
      clearInterval(this.interval)
      this.playTime = '00:00'
      this.interval = null
    }
  },

  computed: {
    gameButton: function () {
      if (this.interval) {
        return 'NEW GAME'
      } else {
        return 'START GAME'
      }
    }
  }

}
</script>

<style>

</style>