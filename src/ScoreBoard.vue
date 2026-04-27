<template>
  <div class="card">
    <h2>🏆 即時排行榜（壓軸版）</h2>

    <!-- 🔥 第一名粒子特效 -->
    <div v-if="sorted.length" class="particles">
      <span v-for="n in 20" :key="n"></span>
    </div>

    <!-- 加分提示 -->
    <div v-if="toast" class="toast">{{ toast }}</div>

    <!-- 排行榜動畫 -->
    <TransitionGroup name="list" tag="div">
      <div
        v-for="(team,index) in sorted"
        :key="team.id"
        :class="['team', rankClass(index), { bump: team.bump }]"
      >
        <div class="rank">{{ index+1 }}</div>

        <div class="name">{{ team.name }}</div>

        <div class="bar">
          <div
            class="fill"
            :style="{ width: percent(team.score)+'%' }"
          ></div>
        </div>

        <div class="score">{{ team.score }}</div>

        <div class="actions">
          <button @click="add(team.id,1)">+1</button>
          <button @click="add(team.id,5)">+5</button>
          <button @click="add(team.id,-1)">-1</button>
          <button @click="remove(team.id)">❌</button>
        </div>
      </div>
    </TransitionGroup>

    <!-- 新增 -->
    <div class="add-box">
      <input v-model="newName" placeholder="新增小組" />
      <button @click="addTeam">新增</button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import { playSound } from '../../utils/sound'

const teams = ref([
  { id:1, name:'第一組', score:0, bump:false },
  { id:2, name:'第二組', score:0, bump:false }
])

const newName = ref('')
const toast = ref('')

// 排序
const sorted = computed(()=>{
  return [...teams.value].sort((a,b)=>b.score-a.score)
})

// 最大值
const maxScore = computed(()=>{
  return Math.max(...teams.value.map(t=>t.score),1)
})

// 百分比
const percent = (score)=>{
  return (score / maxScore.value) * 100
}

// 排名樣式
const rankClass = (i)=>{
  if(i===0) return 'gold'
  if(i===1) return 'silver'
  if(i===2) return 'bronze'
  return ''
}

// 加分
const add = (id,val)=>{
  const t = teams.value.find(t=>t.id===id)
  if(t){
    t.score += val
    t.bump = true
    playSound('win')

    toast.value = `${t.name} ${val>0?'+':''}${val}`
    setTimeout(()=> toast.value='',1500)

    setTimeout(()=> t.bump=false,300)
  }
}

// 刪除
const remove = (id)=>{
  teams.value = teams.value.filter(t=>t.id!==id)
}

// 新增
const addTeam = ()=>{
  if(!newName.value) return

  teams.value.push({
    id: Date.now(),
    name: newName.value,
    score: 0,
    bump:false
  })

  newName.value=''
}
</script>

<style scoped>
.team {
  display: grid;
  grid-template-columns: 40px 80px 1fr 60px auto;
  gap: 10px;
  align-items: center;
  margin: 12px 0;
  padding: 12px;
  border-radius: 12px;
  background: #f5f5f5;
  transition: 0.4s;
}

/* 🥇🥈🥉 */
.gold {
  background: linear-gradient(135deg, gold, orange);
  transform: scale(1.05);
  box-shadow: 0 0 25px gold;
}
.silver {
  background: linear-gradient(135deg, #ddd, #fff);
}
.bronze {
  background: linear-gradient(135deg, #cd7f32, #e6a86b);
}

/* 分數條 */
.bar {
  height: 12px;
  background: #ddd;
  border-radius: 10px;
  overflow: hidden;
}
.fill {
  height: 100%;
  background: linear-gradient(135deg, #667eea, #764ba2);
  transition: width 0.5s;
}

/* 加分動畫 */
.bump {
  animation: pop 0.3s;
}
@keyframes pop {
  50% { transform: scale(1.15); }
}

/* 排名動畫 */
.list-move {
  transition: transform 0.5s;
}

/* 🔥 粒子特效 */
.particles {
  position: absolute;
  width: 100%;
  height: 100px;
  pointer-events: none;
}

.particles span {
  position: absolute;
  width: 6px;
  height: 6px;
  background: gold;
  border-radius: 50%;
  animation: float 2s infinite;
}

.particles span:nth-child(odd) {
  background: orange;
}

@keyframes float {
  0% { transform: translateY(0); opacity:1; }
  100% { transform: translateY(-80px); opacity:0; }
}

/* toast */
.toast {
  background: black;
  color: white;
  padding: 8px;
  border-radius: 10px;
  text-align: center;
  margin-bottom: 10px;
}

/* 新增 */
.add-box {
  margin-top: 15px;
}
</style>