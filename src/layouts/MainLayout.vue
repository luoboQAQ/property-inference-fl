<script setup>
import { ref } from 'vue'
// 引入 echarts 核心模块，核心模块提供了 echarts 使用必须要的接口。
import { use } from "echarts/core";
// 引入柱状图图表，图表后缀都为 Chart
import { BarChart, LineChart } from 'echarts/charts';
// 引入 Canvas 渲染器，注意引入 CanvasRenderer 或者 SVGRenderer 是必须的一步
import { CanvasRenderer } from "echarts/renderers";
// 标签自动布局、全局过渡动画等特性
import { LabelLayout, UniversalTransition } from 'echarts/features';
// 引入提示框，标题，直角坐标系，数据集，内置数据转换器组件，组件后缀都为 Component
import {
  TitleComponent,
  TooltipComponent,
  GridComponent,
  DatasetComponent,
  TransformComponent,
  LegendComponent
} from 'echarts/components';
import VChart from "vue-echarts";

import data from '@/static/data.json'

// 注册必须的组件
use([
  CanvasRenderer,
  BarChart,
  TitleComponent,
  TooltipComponent,
  GridComponent,
  DatasetComponent,
  TransformComponent,
  LabelLayout,
  UniversalTransition,
  LineChart,
  LegendComponent
]);

const load = ref(false)
const option = ref({
  animationDuration: 10000,
  dataset: [
    {
      id: "dataset_raw",
      source: data
    },
    {
      id: "dataset_filtered",
      fromDatasetId: "dataset_raw",
      transform: {
        type: "filter",
        config: {
          and: [
            { dimension: "task", '=': "gender" },
            { dimension: "attr", '=': "race" },
            { dimension: "property", '=': "2" },
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
    type: 'category'
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
      datasetId: "dataset_filtered",
      showSymbol: false,
      name: "准确率",
      endLabel: {
        show: true,
        formatter: function (params) {
          return params.value[4] + '%';
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
      datasetId: "dataset_filtered",
      showSymbol: false,
      name: "p值",
      endLabel: {
        show: true,
        formatter: function (params) {
          return params.value[5] + '%';
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
})

const image = ref(null)

function test() {
  let find = false
  let attr = attrs.value.split('-')[0]
  let property = Number(attrs.value.split('-')[1])
  // console.log(task.value,attr, property)
  for (let index = 0; index < data.length; index++) {
    const element = data[index];
    if(element[0]===task.value && element[1]===attr && element[2]===property) {
      find = true
      break
    }
  }
  if(!find) {
    alert('没有找到数据')
    return
  }
  option.value.dataset[1].transform.config.and = [
            { dimension: "task", '=': task.value },
            { dimension: "attr", '=': attr },
            { dimension: "property", '=': property },
          ]
  image.value.clear()
  image.value.setOption(option.value)
}

const task = ref(null)
const attrs = ref(null)
const options = [
  {
    label: '性别', value: 'gender', children: [
      { label: '男性', value: 'gender-0' },
      { label: '女性', value: 'gender-1' }
    ]
  },
  {
    label: '笑脸', value: 'smile', children: [
      { label: '无笑脸', value: 'smile-0' },
      { label: '有笑脸', value: 'smile-1' }
    ]
  },
  {
    label: '种族', value: 'race', children: [
      { label: '亚洲人', value: 'race-0' },
      { label: '白人', value: 'race-1' },
      { label: '黑人', value: 'race-2' },
    ]
  },
  {
    label: '眼睛', value: 'glasses', children: [
      { label: '有眼镜', value: 'glasses-0' },
      { label: '太阳眼镜', value: 'glasses-1' },
      { label: '无眼镜', value: 'glasses-2' },
    ]
  },
  {
    label: '年龄', value: 'age', children: [
      { label: '婴儿', value: 'age-0' },
      { label: '孩子', value: 'age-1' },
      { label: '青年', value: 'age-2' },
      { label: '中年人', value: 'age-3' },
      { label: '老年人', value: 'age-4' },
    ]
  },
  {
    label: '发色', value: 'hair', children: [
      { label: '黑发', value: 'hair-0' },
      { label: '金发', value: 'hair-1' },
      { label: '棕发', value: 'hair-2' },
      { label: '秃头', value: 'hair-3' },
    ]
  }
]
</script>

<template>
  <n-grid>
    <n-gi span="24">
      <v-chart :option="option" style="height: 600px;" :loading="load" ref="image" />
    </n-gi>
  </n-grid>
  <n-grid>
    <n-gi span="6">
      <n-select v-model:value="task" :options="[
        { label: '性别', value: 'gender' },
        { label: '笑脸', value: 'smile' },
        { label: '种族', value: 'race' },
        { label: '眼睛', value: 'glasses' },
        { label: '年龄', value: 'age' },
        { label: '发色', value: 'hair' }
      ]" />
    </n-gi>
    <n-gi span="6">
      <n-cascader v-model:value="attrs" check-strategy="child" :options="options" />
    </n-gi>
    <n-gi span="6">
      <n-button @click="test()">切换加载状态</n-button>
    </n-gi>
  </n-grid>
</template>

<style scoped></style>
