<script setup lang="ts">
import FillBar from "./FillBar.vue"
import { ref, computed } from 'vue'

const startTime = ref<number|null>(null)
const elapsed = ref<number|null>(null)
const isRunning = ref(false)
const timer = ref<number|null>(null)

const ONE_MINUTE = 60000
const INITIAL_TIME = ONE_MINUTE*14
const PHASE_1_TIME = ONE_MINUTE*4.5
const PHASE_2_TIME = ONE_MINUTE*7.5
const PHASE_3_TIME = ONE_MINUTE*11
const PHASE_4_TIME = ONE_MINUTE*14

type state = "INITIAL" | "PHASE_1" | "PHASE_2" | "PHASE_3" | "PHASE_4" | "END"

const currentState = computed(() => {
  if (timer.value === null) {
    return "INITIAL"
  }
  if ((elapsed.value ?? 0) < PHASE_1_TIME) {
    return "PHASE_1"
  }
  if ((elapsed.value ?? 0) < PHASE_2_TIME) {
    return "PHASE_2"
  }
  if ((elapsed.value ?? 0) < PHASE_3_TIME) {
    return "PHASE_3"
  }
  if ((elapsed.value ?? 0) < PHASE_4_TIME) {
    return "PHASE_4"
  }
  return "END"
})

const displayText = computed(() => {
  switch (currentState.value) {
    case "INITIAL": return "Press Start to start day"
    case "PHASE_1": return "Ring will start shrinking in"
    case "PHASE_2": return "Ring is shrinking, will stop in"
    case "PHASE_3": return "Ring will start shrinking in"
    case "PHASE_4": return "Ring is shrinking, will reach boss area in"
    case "END": return "Ring fully closed, good luck with your boss!"
  }
})

const formattedTime = computed(() => {
  if (!startTime.value) return '04:30'

  var remaining = 0
  switch (currentState.value) {
    case "PHASE_1": remaining = Math.max(0, PHASE_1_TIME - (elapsed.value ?? 0))
      break
    case "PHASE_2": remaining = Math.max(0, PHASE_2_TIME - (elapsed.value ?? 0))
      break
    case "PHASE_3": remaining = Math.max(0, PHASE_3_TIME - (elapsed.value ?? 0))
      break
    case "PHASE_4": remaining = Math.max(0, PHASE_4_TIME - (elapsed.value ?? 0))
      break
  }

  const seconds = Math.floor((remaining / 1000) % 60)
  const minutes = Math.floor(remaining / 60000)

  return `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`
})

const dayProgress = computed(() => {
  return Math.min((elapsed.value ?? 0) * 100 / INITIAL_TIME,100)
})

function start() {
  if (timer.value) {
    clearInterval(timer.value)
    timer.value = null
  }
    isRunning.value = true
    startTime.value = Date.now()
    elapsed.value = Date.now() - (startTime.value ?? 0)
    timer.value = setInterval(() => {
      elapsed.value = Date.now() - (startTime.value ?? 0)
    }, 100)

}


function reset() {
  if (timer.value) {
    clearInterval(timer.value)
    timer.value = null
  }
  isRunning.value = false
  startTime.value = null
}

</script>

<template>
  <div class="timer-container">
    <FillBar :fill="dayProgress"/>

    <div class="text-display smol">{{ displayText }}</div>
    <div class="text-display">{{ formattedTime }}</div>


    <div class="controls">
      <button
        v-if="['INITIAL','END'].includes(currentState)"
        @click="start"
        class="control-button"
      >
        Start
      </button>
      <button
        v-if="!['INITIAL','END'].includes(currentState)"
        @click="reset"
        class="control-button"
      >
        Reset
      </button>
    </div>
  </div>
</template>

<style scoped>
.timer-container {
  text-align: center;
  width: 75vw;
}

.text-display {
  font-size: 48px;
  margin: 1rem;
  font-family: monospace;
}
.smol {
  font-size: 32px;
}
.controls {
  display: flex;
  gap: 10px;
  justify-content: center;
}

.control-button {
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  border: 1px solid #938461;
  border-radius: 4px;
  background-color: #736f9a;
  color: #ffffff;
}

.control-button:hover {
  background-color: #5b509e;
}
</style>
