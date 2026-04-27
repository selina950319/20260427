<template>
  <div class="card">
    <h2>🎲 抽籤</h2>

    <h1>{{ result || '點擊抽籤' }}</h1>

    <button @click="draw">抽</button>

    <hr />

    <input v-model="newName" placeholder="新增名字" />
    <button @click="add">新增</button>

    <ul>
      <li v-for="(n,i) in names" :key="i">
        {{ n }}
        <button @click="remove(i)">❌</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { playSound } from '../../utils/sound'

const names = ref(['小明','小華'])
const result = ref('')
const newName = ref('')

const add = ()=>{
  if(newName.value){
    names.value.push(newName.value)
    newName.value=''
  }
}

const remove = (i)=>{
  names.value.splice(i,1)
}

const draw = ()=>{
  if(!names.value.length) return
  const i = Math.floor(Math.random()*names.value.length)
  result.value = names.value[i]

  // 🔊 中獎音效
  playSound('win')
}
</script>