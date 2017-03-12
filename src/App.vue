<template>
  <div id="app">
    <div id="timer">
      <div id="settings" class="container">
        <h3 v-on:click="decPom">-</h3>
        <h2>{{ pomTime }}</h2>
        <h3 v-on:click="incPom">+</h3>
        <div id="spacer"></div>
        <h3 v-on:click="decBreak">-</h3>
        <h2>{{ breakTime }}</h2>
        <h3 v-on:click="incBreak">+</h3>
      </div>
      <div id="clock">
        <h1 v-on:click="timeToggle">{{ timeLeftFormatted }}</h1>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'app',
  data () {
    return {
      timeFor: 'pom',
      breakTime: 5,
      pomTime: 20,
      timerRunning: false,
      timerCode: ''
    }
  },
  computed: {
    timeLeftFormatted: function () {
      const min = this.timeLeft / 60
      const sec = this.timeLeft % 60
      const trimSec = ('0' + sec.toString()).slice(-2)
      return min.toString() + ':' + trimSec
    },
    timeLeft: function () {
      return this.pomTime * 60
    }
  },
  methods: {
    incPom: function () {
      this.pomTime++
    },
    decPom: function () {
      this.pomTime--
    },
    incBreak: function () {
      this.breakTime++
    },
    decBreak: function () {
      this.breakTime--
    },
    timeToggle: function () {
      if (this.timerRunning === true) {
        this.timerRunning = false
        clearTimeout(this.timerCode)
      } else {
        this.timerRunning = true
      }
    },
    timeTick: function () {
      'do something here'
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}


#spacer {
  width: 100px;
}

.container {
  display: flex;
  justify-content: center;
}
</style>
