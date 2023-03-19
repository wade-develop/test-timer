<script setup>
import DurationInput from './components/DurationInput.vue'
import TimerItem from './components/TimerItem.vue'
</script>

<template>
  <div class="wrapper">
    <DurationInput @set-timer="addTimer" />
    <TimerItem v-for="timer in timers" :key="timer.start" :timer="timer" @delete-timer="deleteTimer" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      timers: [],
    };
  },
  methods: {
    addTimer(duration) {
      this.timers.push({
        start: Date.now(),
        duration
      });
    },
    deleteTimer(timestamp) {
      this.timers = this.timers.filter(({ start }) => start !== timestamp)
    }
  }
}
</script>

<style scoped>
  .wrapper {
    padding: 20px
  }
</style>
