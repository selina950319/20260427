<template>
  <div class="card">
    <h2>⏱ 計時器</h2>

    <h1 :class="{ warn: time <= 5 && time > 0 }">
      {{ time }}
    </h1>

    <input v-model.number="input" type="number" />
    <button @click="setTime">設定</button>

    <br /><br />

    <button @click="start">開始</button>
    <button @click="reset">重置</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { playSound } from '../../utils/sound'

const time = ref(0)
const input = ref(10)
let timer = null

const setTime = () => {
  time.value = input.value
}

const start = () => {
  if (timer || time.value <= 0) return

  timer = setInterval(() => {
    if (time.value > 0) {
      time.value--

      if (time.value <= 5 && time.value > 0) {
        playSound('countdown')
      }

    } else {
      clearInterval(timer)
      timer = null
      playSound('alarm')
    }
  }, 1000)
}

const reset = () => {
  clearInterval(timer)
  timer = null
  time.value = 0
}
</script>

<style scoped>
.warn {
  color: red;
  animation: blink 0.5s infinite;
}

@keyframes blink {
  50% { opacity: 0; }
}
</style>