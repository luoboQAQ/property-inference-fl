<script setup>
import { ref } from 'vue'
import EChartImage from '@/components/EChartImage.vue'
import ModelControl from '@/components/ModelControl.vue'
import ModelInfo from '@/components/ModelInfo.vue'
import ScoreTable from '@/components/ScoreTable.vue'
import DataDistribution from '@/components/DataDistribution.vue'

const load = ref(false)
const image = ref(null)
const score = ref(null)
const distribution = ref(null)

function update(task, attr, property, tittle) {
  load.value = true
  setTimeout(() => {
    image.value.update(task, attr, property, tittle)
    score.value.update(task, attr, property)
    distribution.value.update(task, attr, property)
    load.value = false
  }, 2000)
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
  <n-grid x-gap="12" style="margin-top: 5px">
    <n-gi span="18">
      <e-chart-image :load="load" ref="image" />
    </n-gi>
    <n-gi span="6">
      <score-table :load="load" ref="score" />
      <br />
      <data-distribution :load="load" ref="distribution" />
    </n-gi>
  </n-grid>
</template>

<style scoped>
.title {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 300px;
}
</style>
