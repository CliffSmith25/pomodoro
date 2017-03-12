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
      <div id="type">
        <h2 v-on:click="toggleTimeType">{{ timeType }}</h2>
      </div>
      <audio ref="alarm" src="/static/alarm.mp3"></audio>
      <h4><a href="https://github.com/CliffSmith25/pomodoro">View the source code here</a></h4>
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
      pomTime: 25,
      timerRunning: false,
      timerCode: '',
      secondsElapsed: 0,
      timeType: 'Pomodoro'
    }
  },
  computed: {
    timeLeftFormatted: function () {
      const min = Math.floor(this.timeLeft / 60)
      const sec = this.timeLeft % 60
      const trimSec = ('0' + sec.toString()).slice(-2)
      return min.toString() + ':' + trimSec
    },
    timeLeft: function () {
      if (this.timeType === 'Pomodoro') {
        return this.pomTime * 60 - this.secondsElapsed
      } else {
        return this.breakTime * 60 - this.secondsElapsed
      }
    }
  },
  methods: {
    incPom: function () {
      if (this.timeType === 'Pomodoro' && this.timerRunning === true) {
        this.timeToggle()
        this.secondsElapsed = 0
      }
      this.pomTime++
    },
    decPom: function () {
      if (this.timeType === 'Pomodoro' && this.timerRunning === true) {
        this.timeToggle()
        this.secondsElapsed = 0
      }
      if (this.pomTime !== 1) this.pomTime--
    },
    incBreak: function () {
      if (this.timeType === 'Break' && this.timerRunning === true) {
        this.timeToggle()
        this.secondsElapsed = 0
      }
      this.breakTime++
    },
    decBreak: function () {
      if (this.timeType === 'Break' && this.timerRunning === true) {
        this.timeToggle()
        this.secondsElapsed = 0
      }
      if (this.breakTime !== 1) this.breakTime--
    },
    timeToggle: function () {
      if (this.timerRunning === true) {
        this.timerRunning = false
        clearTimeout(this.timerCode)
      } else {
        this.timerRunning = true
        this.timerCode = setInterval(this.timeTick, 1000)
      }
    },
    timeTick: function () {
      this.secondsElapsed++
      if (this.timeLeft === 0) {
        this.alarmSound()
        this.toggleTimeType()
      }
    },
    toggleTimeType: function () {
      this.timeToggle()
      this.secondsElapsed = 0
      if (this.timeType === 'Break') {
        this.timeType = 'Pomodoro'
      } else {
        this.timeType = 'Break'
      }
    },
    alarmSound: function () {
      this.$refs.alarm.play()
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
  -moz-user-select: none;
  -khtml-user-select: none;
  -webkit-user-select: none;
}

h1, h2, h3 {
  cursor: pointer;
}

h1 {
  font-size: 5em;
  margin-top: 20px;
}

h2 {
  font-size: 4em;
  align-self: center;
  margin-left: 4px;
  margin-right: 4px;
}

h3 {
  font-size: 3em;
  align-self: center;
}

#spacer {
  width: 80px;
}

.container {
  display: flex;
  justify-content: center;
}
</style>
