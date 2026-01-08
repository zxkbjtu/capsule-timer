<script setup lang="ts">
import { ref, computed } from 'vue'
import { Play, Square } from 'lucide-vue-next'

// --- 状态定义 ---
const status = ref('idle') 
const minutes = ref(60)
const seconds = ref(0)
const timerInterval = ref<any>(null)

// 【新增】用于记忆开始前设定的时间
const savedMinutes = ref(60)
const savedSeconds = ref(0)

// --- 辅助函数 ---
const formatNum = (num: number) => num.toString().padStart(2, '0')

// --- 交互逻辑 ---
const handleWheel = (type, e: WheelEvent) => {
  if (status.value !== 'idle') return
  const delta = e.deltaY > 0 ? -1 : 1
  
  if (type === 'min') {
    let newVal = minutes.value + delta
    if (newVal > 99) newVal = 0
    if (newVal < 0) newVal = 99
    minutes.value = newVal
  } else {
    let newVal = seconds.value + delta
    if (newVal > 59) newVal = 0
    if (newVal < 0) newVal = 59
    seconds.value = newVal
  }
}

// --- 核心逻辑 ---
const toggleTimer = () => {
  if (status.value === 'idle') {
    startTimer()
  } else {
    resetTimer()
  }
}

const startTimer = () => {
  if (minutes.value === 0 && seconds.value === 0) return
  
  // 【关键修改 1】开始计时前，先“记住”当前设定的时间
  savedMinutes.value = minutes.value
  savedSeconds.value = seconds.value
  
  status.value = 'running'
  
  timerInterval.value = setInterval(() => {
    if (seconds.value > 0) {
      seconds.value--
    } else {
      if (minutes.value > 0) {
        minutes.value--
        seconds.value = 59
      } else {
        triggerAlarm()
      }
    }
  }, 1000)
}

const resetTimer = () => {
  clearInterval(timerInterval.value)
  status.value = 'idle'
  
  // 【关键修改 2】停止时，恢复到最初记录的时间
  minutes.value = savedMinutes.value
  seconds.value = savedSeconds.value
}

const triggerAlarm = () => {
  clearInterval(timerInterval.value)
  status.value = 'alarm'
  
  new Notification('时间到！', {
    body: '专注结束，休息一下吧。',
    silent: false
  })
}

// --- 样式计算 ---
const containerClass = computed(() => {
  if (status.value === 'alarm') {
    return 'bg-red-500/90 animate-pulse ring-4 ring-red-300'
  }
  return 'bg-slate-900/90 border border-slate-700/50 shadow-xl backdrop-blur-md'
})
</script>

<template>
  <div class="w-full h-screen flex items-center justify-center overflow-visible">
    
    <div 
      class="relative flex items-center justify-between px-4 pt-1 pb-1.5 rounded-full transition-all duration-300 w-[200px] h-[56px] group"
      :class="containerClass"
      style="-webkit-app-region: drag;"
    >
      
      <div 
        class="flex items-center space-x-1 text-3xl font-mono font-bold text-white tracking-widest cursor-ns-resize leading-none"
        style="-webkit-app-region: no-drag;"
      >
        <div 
          @wheel.prevent="(e) => handleWheel('min', e)"
          :class="{'text-slate-400': status === 'running', 'hover:text-blue-400': status === 'idle'}"
          class="transition-colors w-[42px] text-center select-none"
        >
          {{ formatNum(minutes) }}
        </div>
        
        <div class="text-slate-500 mt-[-2px] select-none">:</div>
        
        <div 
          @wheel.prevent="(e) => handleWheel('sec', e)"
          :class="{'text-slate-400': status === 'running', 'hover:text-blue-400': status === 'idle'}"
          class="transition-colors w-[42px] text-center select-none"
        >
          {{ formatNum(seconds) }}
        </div>
      </div>

      <button 
        @click="toggleTimer"
        class="flex items-center justify-center w-10 h-10 rounded-full bg-slate-700 text-white hover:bg-slate-600 active:scale-95 transition-all outline-none ring-0 border-0 shadow-inner mt-[2px]"
        style="-webkit-app-region: no-drag;"
      >
        <Play v-if="status === 'idle'" :size="20" fill="currentColor" class="ml-1" />
        <Square v-else :size="18" fill="currentColor" />
      </button>

    </div>
  </div>
</template>

<style>
/* 确保 html/body 背景透明，否则会有白色底块 */
html, body, #app {
  background: transparent !important;
  height: 100vh;
  width: 100vw;
  overflow: hidden;
}
</style>