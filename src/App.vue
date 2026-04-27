<template>
  <div>
    <h1 class="title">🎓 教學互動平台</h1>

    <div class="nav">
      <button @click="page='score'">🏆</button>
      <button @click="page='lottery'">🎲</button>
      <button @click="page='timer'">⏱</button>
      <button @click="page='quiz'">📝</button>
    </div>

    <ScoreBoard v-if="page==='score'" />
    <Lottery v-if="page==='lottery'" />
    <Timer v-if="page==='timer'" />
    <Quiz v-if="page==='quiz'" />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import ScoreBoard from './components/tools/ScoreBoard.vue'
import Lottery from './components/tools/Lottery.vue'
import Timer from './components/tools/Timer.vue'
import Quiz from './components/exam/Quiz.vue'

const page = ref('score')
</script>