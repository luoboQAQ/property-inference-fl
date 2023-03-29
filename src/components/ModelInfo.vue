<script setup>
import num from '@/static/num-data.json'

defineProps({
  load: {
    type: Boolean,
    required: true
  }
})
defineExpose({
  update
})

let data = num['gender-race-0']

function update(task, attr, property) {
  data = num[`${task}-${attr}-${property}`]
}
</script>

<template>
  <n-spin :show="load">
    <n-card title="模型参数" style="height: 300px">
      <n-grid>
        <n-gi span="12">
          <n-statistic label="训练集" tabular-nums>
            <n-number-animation :from="0" :to="data[0]" />
            <template #suffix> 组 </template>
          </n-statistic>
        </n-gi>
        <n-gi span="12">
          <n-statistic label="测试集" tabular-nums>
            <n-number-animation :from="0" :to="data[1]" />
            <template #suffix> 组 </template>
          </n-statistic>
        </n-gi>
      </n-grid>
      <n-grid>
        <n-gi span="12">
          <n-statistic label="攻击者有标签数据" tabular-nums>
            <n-number-animation :from="0" :to="data[2]" />
            <template #suffix> 组 </template>
          </n-statistic>
        </n-gi>
        <n-gi span="12">
          <n-statistic label="攻击者无标签数据" tabular-nums>
            <n-number-animation :from="0" :to="data[3]" />
            <template #suffix> 组 </template>
          </n-statistic>
        </n-gi>
      </n-grid>
    </n-card>
    <template #description> loading </template>
  </n-spin>
</template>

<style scoped></style>
