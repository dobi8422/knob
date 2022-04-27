<script setup>
import { ref, onMounted, computed, defineEmits } from 'vue'

const emit = defineEmits({
  rotateValue: Number
})

// 自訂最大最小值
const minValue = ref(0)
const maxValue = ref(100)

// 設定起點位置(角度)
const initial = ref(90)

const nowdeg = ref(0)
const value = computed(() => {
  const percent = Math.round(nowdeg.value * (maxValue.value - minValue.value) / 360 + minValue.value)
  emit('rotateValue', percent)
  return percent
})

const calculateDeg = e => {
  const x1 = window.innerWidth / 2
  const y1 = window.innerHeight / 2
  const x2 = e.clientX
  const y2 = e.clientY

  const deltaX = x1 - x2
  const deltaY = y1 - y2

  const rad = Math.atan2(deltaY, deltaX)
  const deg = rad * (180 / Math.PI)
  return deg
}

onMounted(() => {
  const container = document.querySelector('.container')
  const knob = document.querySelector('.knob')
  knob.style.transform = `rotate(${initial.value}deg)`
  container.addEventListener('mousedown', () => {
    const rotate = e => {
      const result = Math.floor(calculateDeg(e) - 90 - initial.value)
      nowdeg.value = result > 0
        ? result - 1
        : result + 360
    }
    container.addEventListener('mousemove', rotate)
    container.addEventListener('mouseup', () => {
      container.removeEventListener('mousemove', rotate)
    })
  })
})
</script>

<template>
  <div class="container">
    <div class="centerCircle">{{ value }}</div>
    <div class="knob" :style="{background: `conic-gradient(teal 0, teal ${nowdeg}deg, rgb(200, 200, 200) ${nowdeg}deg, rgb(200, 200, 200))`}"></div>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 100vw;
  height: 100vh;
  user-select: none;
}
.centerCircle {
  width: 90px;
  height: 90px;
  background: white;
  border-radius: 50%;
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1;
}
.knob {
  width: 125px;
  height: 125px;
  border-radius: 50%;
  position: absolute;
  background: none;
  box-shadow: 5px 5px 8px rgb(200, 200, 200);
}
</style>
