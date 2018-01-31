<template>
  <div id="app">
   
   <section class="timerDisplay">
     <div>
       <button @click="minusTotalTime">-</button>
       <div>
         <p>Total Time</p>
         <p>{{ overallTime }}</p>
       </div>
       <button @click="addTotalTime">+</button>
     </div>
     <div v-for="interval in numberOfIntervals" :key="interval.id">
       <button @click="decreaseInterval(interval)">-</button>
       <div>
         <p>Interval {{ interval.id }}</p>
         <p>{{ interval.length }}</p>
       </div>
       <button @click="increaseInterval(interval)">+</button>
     </div>
   </section>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      overallTime: 30,
      numberOfIntervals: [
        {id: 1, length: '0'},
        {id: 2, length: '0'}
      ]
    }
  },
  methods: {
    addTotalTime() {
      this.overallTime += 1;
    },
    minusTotalTime() {
      this.overallTime -= 1;
    },
    increaseInterval(interval) {
      interval.length++;
    },
    decreaseInterval() {
      this.currentInterval -= 1;
    }
  }
}
</script>

<style scoped>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
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
  margin: 40px 0px;
  font-size: 24px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.timerDisplay > div {
  width: 300px;
  margin: 10px 0px;
  border: 1px solid #cccccc;
  padding: 20px 0px;
  display: flex;
  justify-content: space-around;  
}

</style>
