<script setup lang="ts">

const props = defineProps<{
  fill: number
}>()

</script>

<template>
  <div class="bar-container">
  <div class="boxes">
    <div class="phase1"></div>
    <div class="phase2"></div>
    <div class="phase3"></div>
    <div class="phase4"></div>
  </div>
  <div class="bar" :style="{ width: fill + '%' }"></div>
  </div>
</template>

<style scoped>
.bar-container {
  position: relative;
}

.boxes {
  position: relative;
  display: grid;
  width: 100%;
  height: 80px;
  grid-template-columns: 9fr 6fr 7fr 6fr;

  div {
    border: 1px solid #dcb74e;
  }
}

.bar {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  z-index: -1;
  background-color: #938461;
}
</style>
