<template>
  <div class="card">
    <h2>📝 測驗</h2>

    <!-- 題目編輯 -->
    <div v-for="(q,i) in qs" :key="i">
      <input v-model="q.text" placeholder="題目" />

      <div v-for="(o,j) in q.opts" :key="j">
        <input v-model="q.opts[j]" placeholder="選項" />
      </div>

      <input v-model="q.ans" placeholder="答案" />

      <!-- ❌ 刪除題目 -->
      <button @click="remove(i)">刪除題目</button>

      <hr />
    </div>

    <button @click="add">➕ 新增題目</button>

    <!-- 作答 -->
    <h3>作答</h3>

    <div v-for="(q,i) in qs" :key="'a'+i">
      <p>{{ q.text }}</p>

      <label v-for="opt in q.opts" :key="opt">
        <input type="radio" :name="i" :value="opt" v-model="answers[i]" />
        {{ opt }}
      </label>
    </div>

    <button @click="submit">送出</button>

    <h3 v-if="score !== null">分數：{{ score }}</h3>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { playSound } from '../../utils/sound'

const qs = ref([
  { text:'Vue 是什麼？', opts:['框架','語言'], ans:'框架' }
])

const answers = ref({})
const score = ref(null)

const add = ()=>{
  qs.value.push({ text:'', opts:['',''], ans:'' })
}

const remove = (i)=>{
  qs.value.splice(i,1)
}

const submit = ()=>{
  let s = 0

  qs.value.forEach((q,i)=>{
    if(answers.value[i] === q.ans){
      s++

      // 🔊 答對音效
      playSound('correct')
    }
  })

  score.value = s
}
</script>