<script setup>
import { ref } from 'vue'
// 引入 echarts 核心模块，核心模块提供了 echarts 使用必须要的接口。
import { use } from 'echarts/core'
// 引入柱状图图表，图表后缀都为 Chart
import { LineChart } from 'echarts/charts'
// 引入 Canvas 渲染器，注意引入 CanvasRenderer 或者 SVGRenderer 是必须的一步
import { CanvasRenderer } from 'echarts/renderers'
// 标签自动布局、全局过渡动画等特性
import { LabelLayout, UniversalTransition } from 'echarts/features'
// 引入提示框，标题，直角坐标系，数据集，内置数据转换器组件，组件后缀都为 Component
import {
  TitleComponent,
  TooltipComponent,
  GridComponent,
  DatasetComponent,
  TransformComponent,
  LegendComponent
} from 'echarts/components'
import VChart from 'vue-echarts'

import data from '@/static/data.json'

// 注册必须的组件
use([
  CanvasRenderer,
  TitleComponent,
  TooltipComponent,
  GridComponent,
  DatasetComponent,
  TransformComponent,
  LabelLayout,
  UniversalTransition,
  LineChart,
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

const image = ref(null)
const option = {
  animationDuration: 10000,
  dataset: [
    {
      id: 'dataset_raw',
      source: data
    },
    {
      id: 'dataset_filtered',
      fromDatasetId: 'dataset_raw',
      transform: {
        type: 'filter',
        config: {
          and: [
            { dimension: 'task', '=': 'gender' },
            { dimension: 'attr', '=': 'race' },
            { dimension: 'property', '=': '2' }
          ]
        }
      }
    }
  ],
  title: {
    text: '在性别分类模型上推断黑人的准确率'
  },
  tooltip: {
    trigger: 'axis'
  },
  legend: {
    data: ['准确率', 'p值']
  },
  xAxis: {
    type: 'category',
    name: '轮次'
  },
  yAxis: {
    name: '准确率'
  },
  grid: {
    right: 140
  },
  series: [
    {
      type: 'line',
      datasetId: 'dataset_filtered',
      showSymbol: false,
      name: '准确率',
      endLabel: {
        show: true,
        formatter: function (params) {
          return params.value[4] + '%'
        }
      },
      labelLayout: {
        moveOverlap: 'shiftY'
      },
      emphasis: {
        focus: 'series'
      },
      encode: {
        x: 'iteration',
        y: 'test',
        label: ['test acc'],
        itemName: 'iteration',
        tooltip: ['test acc']
      }
    },
    {
      type: 'line',
      datasetId: 'dataset_filtered',
      showSymbol: false,
      name: 'p值',
      endLabel: {
        show: true,
        formatter: function (params) {
          return params.value[5] + '%'
        }
      },
      labelLayout: {
        moveOverlap: 'shiftY'
      },
      emphasis: {
        focus: 'series'
      },
      encode: {
        x: 'iteration',
        y: 'p-test',
        label: ['p-test'],
        itemName: 'iteration',
        tooltip: ['p-test']
      }
    }
  ]
}
function update(task, attr, property, tittle) {
  option.dataset[1].transform.config.and = [
    { dimension: 'task', '=': task },
    { dimension: 'attr', '=': attr },
    { dimension: 'property', '=': property }
  ]
  option.title.text = tittle
  image.value.clear()
  image.value.setOption(option)
}
</script>

<template>
  <v-chart :option="option" style="height: 600px" :loading="load" ref="image" />
</template>

<style scoped></style>
