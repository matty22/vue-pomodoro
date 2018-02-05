<template>
  <div id="app">
   <section class="timerDisplay">
     <div class="intervalBox">
       <button @click="minusTotalTime">-</button>
       <div>
         <p>Total Time</p>
         <p>{{ initialValue / 60 }}:00</p>
       </div>
       <button @click="addTotalTime">+</button>
     </div>
     <section class="intervalSection">
       <div class="intervalBox" v-for="(interval, index) in intervals" :key="interval.id">
        <button @click="decreaseInterval(interval)">-</button>
        <div>
          <p>Interval {{ index + 1 }}</p>
          <p>{{ interval.initialValue / 60 }}:00</p>
        </div>
        <button @click="increaseInterval(interval)">+</button>
       </div>
     </section>
   </section>

   <section class="countdownSection">
     <div>{{ minutes }}:{{ seconds == 0 ? "00" : seconds }}</div>
     <div>{{ intervals[currentInterval].minutes }}:{{ intervals[currentInterval].seconds == 0 ? "00" : intervals[currentInterval].seconds }}</div>
   </section>
   
   <section class="buttonBank">
     <button @click="startTimer">Start</button>
     <button @click="pauseTimer">Pause</button>
     <button @click="resetTimer">Reset</button>
   </section>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      minutes: 25,
      seconds: 0,
      initialValue: 1500,
      overallSeconds: 1500,
      mainTimerId: 0,
      mainTimerIsRunning: false,
      intervals: [
        {initialValue: 60, minutes: 1, seconds: 0, overallSeconds: 60, timerId: 0},
        {initialValue: 60, minutes: 1, seconds: 0, overallSeconds: 60, timerId: 0}
      ],
      currentInterval: 0
    }
  },
  methods: {
    addTotalTime() {
      this.minutes++;
      this.initialValue += 60;
      this.overallSeconds += 60;
    },
    minusTotalTime() {
      this.minutes--;
      this.initialValue -= 60;
      this.overallSeconds -= 60;
    },
    increaseInterval(interval) {
      if (interval.minutes < this.minutes) {
        interval.minutes++;
        interval.initialValue += 60;
        interval.overallSeconds += 60;
      }
    },
    decreaseInterval(interval) {
      if (interval.minutes > 1) {
        interval.minutes--;
        interval.initialValue -= 60;
        interval.overallSeconds -= 60;
      }
    },
    // Function runs on user hitting start button
    startTimer() {
      this.mainTimerIsRunning = true;
      let mainTimer = setInterval(() => { 
        this.mainTimerId = mainTimer;
        this.overallSeconds -= 1;
        this.minutes = Math.floor(this.overallSeconds / 60);
        this.seconds = this.overallSeconds % 60;
        if (this.seconds < 10) {
          this.seconds = "0" + this.seconds;
        }
        if (this.minutes <= 0 && this.seconds <= 0) {
          this.mainTimerIsRunning = false;
          clearInterval(mainTimer);
        }
      }, 1000);
      if (this.currentInterval == 0) {
        this.intervalOneTimer();
      } else {
        this.intervalTwoTimer();
      }
    },
    // TODO: Combine these two interval timers into a single method that takes a parameter as to which interval we are operating on
    intervalOneTimer() {
      if (!this.mainTimerIsRunning) {
        this.intervals[0].minutes = 0;
        this.intervals[0].seconds = 0;
        return false;
      } else {
        let intervalTimer = setInterval(() => {
          this.intervals[0].timerId = intervalTimer;
          this.intervals[0].overallSeconds -= 1;
          this.intervals[0].minutes = Math.floor(this.intervals[0].overallSeconds / 60);
          this.intervals[0].seconds = this.intervals[0].overallSeconds % 60;
          if (this.intervals[0].seconds < 10) {
              this.intervals[0].seconds = "0" + this.intervals[0].seconds;
            }
            if (this.intervals[0].minutes <= 0 && this.intervals[0].seconds <= 0) {
              clearInterval(intervalTimer);
              this.playSound('http://soundbible.com/mp3/Industrial Alarm-SoundBible.com-1012301296.mp3');
              this.intervals[0].minutes = this.intervals[0].initialValue / 60;
              this.intervals[0].overallSeconds = this.intervals[0].initialValue;
              this.currentInterval = 1;
              this.intervalTwoTimer();
            }
        }, 1000);
      }
    },
    // TODO: Combine these two interval timers into a single method that takes a parameter as to which interval we are operating on
    intervalTwoTimer() {
      if (!this.mainTimerIsRunning) {
        this.intervals[1].minutes = 0;
        this.intervals[1].seconds = 0;
        return false;
      } else {
        let intervalTimer = setInterval(() => {
          this.intervals[1].timerId = intervalTimer;
          this.intervals[1].overallSeconds -= 1;
          this.intervals[1].minutes = Math.floor(this.intervals[1].overallSeconds / 60);
          this.intervals[1].seconds = this.intervals[1].overallSeconds % 60;
          if (this.intervals[1].seconds < 10) {
            this.intervals[1].seconds = "0" + this.intervals[1].seconds;
          }
          if (this.intervals[1].minutes <= 0 && this.intervals[1].seconds <= 0) {
            clearInterval(intervalTimer);
            this.playSound('http://soundbible.com/mp3/Ta Da-SoundBible.com-1884170640.mp3');
            this.intervals[1].minutes = this.intervals[1].initialValue / 60;
            this.intervals[1].overallSeconds = this.intervals[1].initialValue;
            this.currentInterval = 0;
            this.intervalOneTimer();
          }
        }, 1000);
      }
    },
    // Clear all intervals on the window by id
    pauseTimer() {
      clearInterval(this.mainTimerId);
      clearInterval(this.intervals[0].timerId);
      clearInterval(this.intervals[1].timerId);
    },
    // Clear all intervals and reset all timers to their intial value
    resetTimer() {
      this.pauseTimer();
      this.overallSeconds = this.initialValue;
      this.minutes = this.initialValue / 60;
      this.seconds = 0;
      this.currentInterval = 0;
      this.intervals[0].overallSeconds = this.intervals[0].initialValue;
      this.intervals[0].minutes = this.intervals[0].initialValue / 60;
      this.intervals[0].seconds = 0;
      this.intervals[1].overallSeconds = this.intervals[0].initialValue;
      this.intervals[1].minutes = this.intervals[0].initialValue / 60;
      this.intervals[1].minutes = 0;
    },
    playSound(sound) {
      let audio = new Audio(sound);
      audio.play();
    }
  }
}
</script>

<style scoped>
body {
  margin: 0;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.timerDisplay {
  margin: 40px 0px 20px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.intervalSection {
  width: 800px;
  display: flex;
  justify-content: space-around;
}

.intervalBox {
  width: 300px;
  margin: 10px 0px;
  border: 1px solid #cccccc;
  padding: 0px 0px;
  display: flex;
  justify-content: space-around;
  align-items: center;  
}

.intervalBox button {
  height: 40px;
}

.countdownSection {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.countdownSection div {
  width: 200px;
  padding: 20px 0px;
  margin: 10px 0px;
  background-color: #000000;
  color: #ff0000;
}

.buttonBank button {
  width: 200px;
  background-color: blue;
  color: #ffffff;
  font-size: 16px;
  border: none;
  padding: 10px 0px;
}

</style>
