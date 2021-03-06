<template>
  <div id="app">
    <h1>Meg's Biking Timer</h1>
   <section class="displayRow">
     <div class="timerSettings">
       <button class="minusButton" @click="minusTotalTime">-</button>
       <div>
         <p>Total Time</p>
         <p>{{ initialValue / 60 }}:00</p>
       </div>
       <button class="plusButton" @click="addTotalTime">+</button>
     </div>
     <div class="timerDisplays">
      <div>{{ minutes < 10 ? "0" + minutes : minutes }}:{{ seconds < 10 ? "0" + seconds : seconds }}</div>
      <div>{{ intervals[currentInterval].minutes < 10 ? "0" + intervals[currentInterval].minutes : intervals[currentInterval].minutes }}:{{ intervals[currentInterval].seconds < 10 ? "0" + intervals[currentInterval].seconds : intervals[currentInterval].seconds }}</div>
     </div>
     <div>
       <div class="timerSettings" v-for="(interval, index) in intervals" :key="interval.id">
        <button class="minusButton" @click="decreaseInterval(interval)">-</button>
        <div>
          <p>Interval {{ index + 1 }}</p>
          <p>{{ interval.initialValue / 60 }}:00</p>
        </div>
        <button class="plusButton" @click="increaseInterval(interval)">+</button>
       </div>
     </div>
   </section>
   
   <section class="buttonRow">
     <button class="plusButton" @click="startTimer">Start</button>
     <button class="pauseButton" @click="pauseTimer">Pause</button>
     <button class="minusButton" @click="resetTimer">Reset</button>
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
        {initialValue: 60, minutes: 1, seconds: 0, overallSeconds: 60, timerId: 0, sound: 'http://soundbible.com/mp3/Industrial Alarm-SoundBible.com-1012301296.mp3'},
        {initialValue: 60, minutes: 1, seconds: 0, overallSeconds: 60, timerId: 0, sound: 'http://soundbible.com/mp3/Ta Da-SoundBible.com-1884170640.mp3'}
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
      // Prevent total time from being lower than the sum of the two intervals
      if (this.minutes <= this.intervals[0].minutes + this.intervals[1].minutes) {
        return false;
      }
      this.minutes--;
      this.initialValue -= 60;
      this.overallSeconds -= 60;
    },
    increaseInterval(interval) {
      // Prevent the sum of the intervals from being increased higher than the total time
      if (this.intervals[0].minutes + this.intervals[1].minutes >= this.minutes) {
        return false;
      }
        interval.minutes++;
        interval.initialValue += 60;
        interval.overallSeconds += 60;
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
        if (this.minutes <= 0 && this.seconds <= 0) {
          this.mainTimerIsRunning = false;
          clearInterval(mainTimer);
        }
      }, 1000);
      this.intervalTimer(this.currentInterval);
    },
    // Function that keeps track of current interval timer
    intervalTimer(interval) {
      if (!this.mainTimerIsRunning) {
        this.intervals[interval].minutes = 0;
        this.intervals[interval].seconds = 0;
        return false;
      } else {
        let intervalTimer = setInterval(() => {
          this.intervals[interval].timerId = intervalTimer;
          this.intervals[interval].overallSeconds -= 1;
          this.intervals[interval].minutes = Math.floor(this.intervals[interval].overallSeconds / 60);
          this.intervals[interval].seconds = this.intervals[interval].overallSeconds % 60;
          if (this.intervals[interval].minutes <= 0 && this.intervals[interval].seconds <= 0) {
            clearInterval(intervalTimer);
            this.playSound(this.intervals[interval].sound);
            this.intervals[interval].minutes = this.intervals[interval].initialValue / 60;
            this.intervals[interval].overallSeconds = this.intervals[interval].initialValue;
            if (this.currentInterval === 0) {
              this.currentInterval = 1;
            } else {
              this.currentInterval = 0;
            }
            this.intervalTimer(this.currentInterval);
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
      this.resetInterval(0);
      this.resetInterval(1);
    },
    resetInterval(interval) {
      this.intervals[interval].overallSeconds = this.intervals[interval].initialValue;
      this.intervals[interval].minutes = this.intervals[interval].initialValue / 60;
      this.intervals[interval].seconds = 0;
    },
    playSound(sound) {
      let audio = new Audio(sound);
      audio.play();
    }
  }
}
</script>

<style scoped>
  @import url('https://fonts.googleapis.com/css?family=Pacifico');

  #app {
    background-color: #607d8b;
    color: #ffffff;
    width: 75%;
    padding: 20px 30px;
    margin: 50px auto;
    border-radius: 10px;
    font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif
  }

  h1 {
    text-align: center;
    padding-bottom: 40px;
    font-family: 'Pacifico', cursive;
  }

  button {
    border: none;
    padding: 10px 15px;
    color: #ffffff;
    border-radius: 10px;
    cursor: pointer;
    position: relative;
    top: 0px;
  }

  button:hover {
    box-shadow: 0 3px 3px 0 #212121;
  }

  button:active {
    box-shadow: 0 1px 1px 0 #212121;
    top: 1px;
  }

  .displayRow {
    display: flex;
    justify-content: space-around;
  }

  .timerSettings {
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
  }

  .timerDisplays {
    display: flex;
    flex-direction: column;
    justify-content: center;
    text-align: center;
    font-size: 48px;
    background-color: #cfd8dc;
    padding: 0px 50px;
    border-radius: 10px;
  }

  .timerDisplays div {
    margin: 20px 0px;
    padding: 10px 20px;
    background-color: #212121;
    border-radius: 10px;
  }

  .timerSettings p {
    padding: 0px 10px;
  }

  .plusButton {
    background-color: #4caf50;
    width: 45px;
  }

  .minusButton {
    background-color: #f44336;
    width: 45px;
  }

  .pauseButton {
    background-color: #2196f3;
  }

  .buttonRow {
    display: flex;
    justify-content: center;
    margin-top: 30px;
    padding-bottom: 40px;
  }

  .buttonRow button {
    width: 125px;
  }

  .buttonRow button:nth-child(2) {
    margin: 0px 15px;
  }

  @media screen and (max-width: 800px) {
    .displayRow {
      flex-direction: column;
    }
  }

</style>
