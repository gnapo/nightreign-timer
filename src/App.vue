<script setup lang="ts">
import  Timer  from './components/Timer.vue'
import {useWakeLock} from "@vueuse/core";
import {reactive} from "vue"
const wakelock = reactive(useWakeLock())
wakelock.request('screen')

</script>

<template>
  <main>
    <Timer />
  </main>
</template>

<style scoped>
main {
  display: flex;
}
</style>
