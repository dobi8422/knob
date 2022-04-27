<script setup>
import { ref, onMounted, computed, defineEmits } from 'vue'

const emit = defineEmits({
  rotateValue: Number
})

// 自訂最大最小值
const minValue = ref(0)
const maxValue = ref(100)

// 設定起點位置(角度)
const initial = ref(0)

const nowdeg = ref(45 + 1)
const value = computed(() => {
  const nowValue = nowdeg.value - 45 > 0
    ? nowdeg.value - 45 - 1
    : nowdeg.value + 360 - 45
  const percent = Math.round(nowValue * (maxValue.value - minValue.value) / 360 + minValue.value)
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
  const knob = document.querySelector('.knob')
  const knob2 = document.querySelector('.knob2')
  knob.style.transform = `rotate(${initial.value + 45}deg)`
  knob.addEventListener('mousedown', () => {
    const rotate = e => {
      const result = Math.floor(calculateDeg(e) - 90 + 45 - initial.value)
      nowdeg.value = result
      knob.style.transform = `rotate(${result + initial.value}deg)`
      if ((result - 45 > 0 ? result - 45 - 1 : result + 360 - 45) > 180) {
        knob2.style.display = 'none'
        // knob.style.borderTop = 'rgb(180, 180, 180)'
        // knob.style.borderRight = 'rgb(180, 180, 180)'
      } else {
        knob2.style.display = 'initial'
        // console.log(knob.style)
        // knob.style.borderTop = 'transparent'
        // knob.style.borderRight = 'transparent'
      }
    }
    knob.addEventListener('mousemove', rotate)
    knob.addEventListener('mouseup', () => {
      knob.removeEventListener('mousemove', rotate)
    })
  })
})
</script>

<template>
  <div class="body">
    <div class="outCircle"></div>
    <div class="centerCircle">{{ value }}</div>
    <div class="knob"></div>
    <div class="knob2"></div>
  </div>
</template>

<style scoped>
.body {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.outCircle {
  width: 145px;
  height: 145px;
  background: rgb(19, 163, 163);
  border-radius: 50%;
  position: absolute;
}
.centerCircle {
  width: 125px;
  height: 125px;
  background: white;
  border-radius: 50%;
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
}
.knob {
  width: 125px;
  height: 125px;
  border-radius: 50%;
  position: absolute;
  background: none;
  border-top: 10px solid rgb(180, 180, 180);
  border-right: 10px solid rgb(180, 180, 180);
  border-bottom: 10px solid transparent;
  border-left: 10px solid transparent;
  z-index: 1;
  /* top: 25px; */
}
.knob2 {
  width: 125px;
  height: 125px;
  border-radius: 50%;
  position: absolute;
  background: none;
  border-top: 10px solid transparent;
  border-right: 10px solid transparent;
  border-bottom: 10px solid rgb(180, 180, 180);
  border-left: 10px solid rgb(180, 180, 180);
  transform: rotate(45deg);
  /* top: 50px; */
}
</style>
