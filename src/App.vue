<template>
  <div id="app">
      <div class="flex items-center justify-center w-screen h-screen">
        <div>
          <div class="flex items-center justify-center mb-4">
            <h1 class="font-bold text-gray-600 text-size">{{ time }}</h1>
          </div>
          <div class="flex items-center justify-center mb-10">
            <button type="button" class="py-3 px-5 text-white bg-blue-500 rounded-lg mr-3 text-lg font-semibold capitalize" @click="clearAll">clear</button>
            <button type="button" class="py-3 px-5 text-white bg-blue-500 rounded-lg mr-3 text-lg font-semibold capitalize" @click="reset">reset</button>
            <button type="button" class="py-3 px-5 text-white bg-blue-500 rounded-lg text-lg font-semibold capitalize" @click="detectAction(running)">{{ running ? 'stop' : 'start' }}</button>
          </div>
          <div>
            <ul class="w-full h-80 overflow-x-auto">
              <li class="text-gray-800 text-lg font-semibold mb-3" v-for="(item, index) in totalStop" :key="index">check Point № {{ index + 1 }} &nbsp; &nbsp; {{ item }} </li>
            </ul>
          </div>
        </div>
      </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      timeBegan: null,
      timeStopped: null,
      stoppedDuration: 0,
      started: null,
      running: false,
      time: '00:00:00.000',
      totalStop: []
    }
  },
  methods: {
    start() {
      if (this.timeBegan === null) {
        this.reset();
        this.timeBegan = new Date();
      }
      if (this.timeStopped !== null) {
        this.stoppedDuration += (new Date() - this.timeStopped);
      }
      this.started = setInterval(this.clockRunning, 10);
    },
    stop(arg) {
      this.running = false;
      this.timeStopped = new Date();
      clearInterval(this.started);
      this.totalStop.push(arg)
    },
    detectAction(arg) {
      if (arg === false) {
        this.start()
        this.running = true
      } else {
        this.stop(this.time)
        this.running = false
      }
    },
    reset() {
      this.running = false;
      clearInterval(this.started);
      this.stoppedDuration = 0;
      this.timeBegan = null;
      this.timeStopped = null;
      this.time = "00:00:00.000";
    },
    clearAll() {
      this.running = false;
      clearInterval(this.started);
      this.stoppedDuration = 0;
      this.timeBegan = null;
      this.timeStopped = null;
      this.time = "00:00:00.000";
      this.totalStop = []
    },
    clockRunning() {
      const currentTime = new Date()
          , timeElapsed = new Date(currentTime - this.timeBegan - this.stoppedDuration)
          , hour = timeElapsed.getUTCHours()
          , min = timeElapsed.getUTCMinutes()
          , sec = timeElapsed.getUTCSeconds()
          , ms = timeElapsed.getUTCMilliseconds();

      this.time =
          this.timeFixer(hour, 2) + ":" +
          this.timeFixer(min, 2) + ":" +
          this.timeFixer(sec, 2) + "." +
          this.timeFixer(ms, 3);
    },
    timeFixer(num, digit) {
      let zero = '';
      for(let i = 0; i < digit; i++) {
        zero += '0';
      }
      return (zero + num).slice(-digit);
    }
  }
}
</script>

<style>

</style>
