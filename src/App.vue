<script setup>
import { ref } from 'vue'
// import Knob from './components/Knob.vue'
import KnobBK from './components/KnobBK.vue'
const KnobValue = ref(0)

const rotateValue = rotateValue => {
  KnobValue.value = rotateValue
}
</script>

<template>
  <div>
    <KnobBK @rotateValue=rotateValue />
    <div>emit: {{ KnobValue }}</div>
  </div>
</template>

<style>
</style>
