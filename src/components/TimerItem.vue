

<template>
  <div class="timer-item">
    <div>
      <span>{{ displayedTime }}</span>
      <span v-if="endingTime">{{ " - " + endingTime }}</span>
    </div>
    <v-btn icon @click="$emit('delete-timer', timer.start)">X</v-btn>
  </div>
</template>

<script>
export default {
  emits: ["delete-timer"],
  props: {
    timer: Object
  },
  data() {
    return {
      remainingTime: null,
      intervalId: null,
      endingTime: null,
    }
  },
  computed: {
    displayedTime() {
      return this.format(this.remainingTime)
    },
  },
  methods: {
    getCurrentTime() {
      const now = new Date();
      const hours = ('0' + now.getHours()).slice(-2);
      const minutes = ('0' + now.getMinutes()).slice(-2);
      const seconds = ('0' + now.getSeconds()).slice(-2);
      return `${hours}.${minutes}.${seconds}`;
    },
    getRemaining() {
      return this.timer.duration + this.timer.start - Date.now()
    },
    finishTimer() {
      this.remainingTime = 0
      this.endingTime = this.getCurrentTime()
      clearInterval(this.intervalId)
    },
    format(ms) {
      const date = new Date(ms)
      const minutes = date.getUTCMinutes().toString().padStart(2, '0')
      const seconds = date.getUTCSeconds().toString().padStart(2, '0')
      const milliseconds = date.getUTCMilliseconds().toString().padStart(3, '0')
      return `${minutes}:${seconds}.${milliseconds}`
    },
    update() {
      const remaining = this.getRemaining()
      remaining > 0 ? this.remainingTime = remaining : this.finishTimer()
    }
  },
  mounted() {
    this.update();
    this.intervalId = setInterval(this.update, 1)
  },
  beforeDestroy() {
    clearInterval(this.intervalId)
  }
}
</script>

<style scoped>
.timer-item {
  background-color: #00000044;
  padding: 10px;
  margin: 5px;
  border-radius: 10px;
  font-size: 30px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>