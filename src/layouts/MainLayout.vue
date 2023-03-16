<script setup>
import { ref } from 'vue'
import EChartImage from '@/components/EChartImage.vue'
import ModelControl from '@/components/ModelControl.vue'
import ModelInfo from '@/components/ModelInfo.vue'

const load = ref(false)
const image = ref(null)

function update(task, attr, property, tittle) {
  load.value = true
  setTimeout(() => {
    image.value.update(task, attr, property, tittle)
    load.value = false
  }, 3000)
}
</script>

<template>
  <n-grid x-gap="12" style="margin-top: 5px">
    <n-gi span="6">
      <model-control @update="update" style="height: 300px; justify-content: center" />
    </n-gi>
    <n-gi span="12">
      <n-gradient-text :size="60" type="success" class="title">
        联邦学习属性推理攻击可视化
      </n-gradient-text>
    </n-gi>
    <n-gi span="6">
      <model-info :load="load" />
    </n-gi>
  </n-grid>
  <br />
  <e-chart-image :load="load" ref="image" />
</template>

<style scoped>
.title {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 300px;
}
</style>
