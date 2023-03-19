<template>
  <div class="input-timer">
    <div class="input-timer__field">
      <div class="input-timer__head">Минуты</div>
      <div class="input-timer__picker">
        <v-btn @click="() => setDuration(-10 * modifier.minutes)">-10</v-btn>
        <v-btn @click="() => setDuration(-1 * modifier.minutes)">-1</v-btn>
        <span class="input-timer__count">{{ minutes }}</span>
        <v-btn @click="() => setDuration(1 * modifier.minutes)">+1</v-btn>
        <v-btn @click="() => setDuration(10 * modifier.minutes)">+10</v-btn>
      </div>
    </div>
    <div class="input-timer__field">
      <div class="input-timer__head">Секунды</div>
      <div class="input-timer__picker">
        <v-btn @click="() => setDuration(-10 * modifier.seconds)">-10</v-btn>
        <v-btn @click="() => setDuration(-1 * modifier.seconds)">-1</v-btn>
        <span class="input-timer__count">{{ seconds }}</span>
        <v-btn @click="() => setDuration(1 * modifier.seconds)">+1</v-btn>
        <v-btn @click="() => setDuration(10 * modifier.seconds)">+10</v-btn>
      </div>
    </div>
    <v-btn @click="setTimer" :disabled="!duration">Создать таймер</v-btn>
  </div>
</template>

<script>
export default {
  emits: ["set-timer"],
  data() {
    return {
      duration: 0,
    }
  },
  computed: {
    threshold() {
      return 1000 * 60 * 60
    },
    modifier() {
      return {
        minutes: 60 * 1000,
        seconds: 1000,
      }
    },
    minutes() {
      return Math.floor(this.duration / this.modifier.minutes)
    },
    seconds() {
      return this.duration / this.modifier.seconds - this.minutes * 60
    }
  },
  methods: {
    setDuration(ms) {
      const currentDuration = this.duration + ms

      switch (true) {
        case currentDuration < 0: return this.duration = 0
        case currentDuration > this.threshold: return this.duration = this.threshold
        default: this.duration = currentDuration
      }
    },
    setTimer() {
      this.$emit("set-timer", this.duration)
      this.duration = 0
    }
  },
}
</script>

<style scoped>
.input-timer {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #ffffff22;
  border-radius: 10px;
  padding: 30px;
}

.input-timer__head {
  min-width: 300px;
  font-size: 30px
}

.input-timer__field {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.input-timer__count {
  padding: 0 10px;
  font-size: 30px
}

.input-timer__picker {
  display: flex;
  align-items: center;
  gap: 5px;
}
</style>
