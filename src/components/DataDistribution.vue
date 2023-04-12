<script setup>
import { ref } from 'vue'
import { use } from 'echarts/core'
import { PieChart } from 'echarts/charts'
import { CanvasRenderer } from 'echarts/renderers'
import { LabelLayout, UniversalTransition } from 'echarts/features'
import {
  GridComponent,
  DatasetComponent,
  TransformComponent,
  LegendComponent
} from 'echarts/components'
import VChart from 'vue-echarts'

import distribution from '@/static/data-distribution.json'

// 注册必须的组件
use([
  CanvasRenderer,
  GridComponent,
  DatasetComponent,
  TransformComponent,
  LabelLayout,
  UniversalTransition,
  PieChart,
  LegendComponent
])

defineProps({
  load: {
    type: Boolean,
    required: true
  }
})

defineExpose({
  update
})

let data = distribution['gender-race-0']

function update(task, attr, property) {
  data = distribution[`${task}-${attr}-${property}`]
  option.value.series[0].data = [
    { value: data[0], name: 'Participant 0' },
    { value: data[1], name: 'Participant 1' },
    { value: data[2], name: 'Attacker' },
    { value: data[3], name: 'Victim' }
  ]
}

const option = ref({
  legend: {
    top: '5%',
    left: 'center'
  },
  series: [
    {
      name: '数据分布',
      type: 'pie',
      radius: ['40%', '70%'],
      avoidLabelOverlap: false,
      itemStyle: {
        borderRadius: 10,
        borderColor: '#fff',
        borderWidth: 2
      },
      label: {
        show: false,
        position: 'center'
      },
      emphasis: {
        label: {
          show: true,
          fontSize: 20,
          fontWeight: 'bold'
        }
      },
      labelLine: {
        show: false
      },
      data: [
        { value: data[0], name: 'Participant 0' },
        { value: data[1], name: 'Participant 1' },
        { value: data[2], name: 'Attacker' },
        { value: data[3], name: 'Victim' }
      ]
    }
  ]
})
</script>

<template>
  <n-card title="数据分布" :loading="load">
    <v-chart :option="option" style="height: 350px" :loading="load" />
  </n-card>
</template>

<style scoped></style>
