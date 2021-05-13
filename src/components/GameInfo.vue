<template>
<div>
  <div class="container d-flex justify-content-between">
    <div>
      <button class="btn btn-secondary me-3" :class="difficultyClass" @click="changeDifficulty">{{ difficulty }}</button>
      <button class="btn btn-primary me-3" @click="startGame">{{ gameButton }}</button>
      <button class="btn btn-warning me-3" @click="stopGame" v-if="interval">Stop Game</button>
      <button class="btn btn-success" @click="submitGame" v-if="interval">Submit Game</button>
    </div>
    <div class="d-inline" v-text="playTime"></div>
  </div>
  <h2 v-text="gameStopMessage"></h2>
</div>
</template>

<script>
export default {
  name: 'GameInfo',

  props: {
    difficulty: String,
    gameStopMessage: String,
  },
  
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
      this.$emit('game-stop')
    },

    submitGame: function () {
      this.$emit('game-submit', this.playTime)
    },

    changeDifficulty: function () {
      this.$emit('change-difficulty')
    }
  },

  computed: {
    gameButton: function () {
      if (this.interval) {
        return 'NEW GAME'
      } else {
        return 'START GAME'
      }
    },
    difficultyClass: function () {
      const result = {
        'btn-muted': false,
        'btn-success': false,
        'btn-danger': false
      }

      if (this.difficulty === 'Easy') {
        result['btn-muted'] = true
      } else if (this.difficulty === 'Medium') {
        result['btn-success'] = true
      } else {
        result['btn-danger'] = true
      }
      return result
    },
  },

  watch: {
    gameStopMessage: function () {
      if (this.gameStopMessage != '' && this.gameStopMessage != 'Wrong Answer') {
        clearInterval(this.interval)
        this.playTime = '00:00'
        this.interval = null
      }
    }
  }
}
</script>

<style>

</style>