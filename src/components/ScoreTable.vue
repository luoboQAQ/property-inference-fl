<script setup>
import { Info16Regular } from '@vicons/fluent'

import score from '@/static/score-data.json'

defineProps({
  load: {
    type: Boolean,
    required: true
  }
})
defineExpose({
  update
})

let data = score['gender-race-0']

function update(task, attr, property) {
  data = score[`${task}-${attr}-${property}`]
}
</script>

<template>
  <n-spin :show="load">
    <n-card title="评估结果">
      <n-table :single-line="false" style="text-align: center">
        <thead>
          <tr>
            <th></th>
            <th>
              <div style="display: flex; justify-content: center; align-items: center">
                精确率
                <n-popover trigger="hover">
                  <template #trigger>
                    <n-icon size="20">
                      <Info16Regular />
                    </n-icon>
                  </template>
                  <p>被分为正例的示例中实际为正例的比例</p>
                  <p>公式:TP/(TP+FP) TP:真正例 FP:假正例</p>
                </n-popover>
              </div>
            </th>
            <th>
              <div style="display: flex; justify-content: center; align-items: center">
                召回率
                <n-popover trigger="hover">
                  <template #trigger>
                    <n-icon size="20">
                      <Info16Regular />
                    </n-icon>
                  </template>
                  <p>实际为正的样本中，被分类器判定为正例的样本所占的比例</p>
                  <p>公式:TP/(TP+FN) TP:真正例 FN:假反例</p>
                </n-popover>
              </div>
            </th>
            <th>
              <div style="display: flex; justify-content: center; align-items: center">
                F1分数
                <n-popover trigger="hover">
                  <template #trigger>
                    <n-icon size="20">
                      <Info16Regular />
                    </n-icon>
                  </template>
                  <p>精确率和召回率的调和平均数</p>
                </n-popover>
              </div>
            </th>
            <th>样本数</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>0.0</td>
            <td>{{ data[0] }}</td>
            <td>{{ data[1] }}</td>
            <td>{{ data[2] }}</td>
            <td>{{ data[3] }}</td>
          </tr>
          <tr>
            <td>1.0</td>
            <td>{{ data[4] }}</td>
            <td>{{ data[5] }}</td>
            <td>{{ data[6] }}</td>
            <td>{{ data[7] }}</td>
          </tr>
          <tr>
            <td>
              <div style="display: flex; justify-content: center; align-items: center">
                微平均
                <n-popover trigger="hover">
                  <template #trigger>
                    <n-icon size="20">
                      <Info16Regular />
                    </n-icon>
                  </template>
                  <p>不分类别进行统计建立全局混淆矩阵，然后计算相应的指标</p>
                </n-popover>
              </div>
            </td>
            <td>{{ data[8] }}</td>
            <td>{{ data[9] }}</td>
            <td>{{ data[10] }}</td>
            <td>{{ data[11] }}</td>
          </tr>
          <tr>
            <td>
              <div style="display: flex; justify-content: center; align-items: center">
                宏平均
                <n-popover trigger="hover">
                  <template #trigger>
                    <n-icon size="20">
                      <Info16Regular />
                    </n-icon>
                  </template>
                  <p>每个类型的P、R的算术平均</p>
                </n-popover>
              </div>
            </td>
            <td>{{ data[12] }}</td>
            <td>{{ data[13] }}</td>
            <td>{{ data[14] }}</td>
            <td>{{ data[15] }}</td>
          </tr>
          <tr>
            <td>
              <div style="display: flex; justify-content: center; align-items: center">
                权重平均
                <n-popover trigger="hover">
                  <template #trigger>
                    <n-icon size="20">
                      <Info16Regular />
                    </n-icon>
                  </template>
                  <p>每一个类别样本数量在所有类别的样本总数的占比作为权重</p>
                </n-popover>
              </div>
            </td>
            <td>{{ data[16] }}</td>
            <td>{{ data[17] }}</td>
            <td>{{ data[18] }}</td>
            <td>{{ data[19] }}</td>
          </tr>
          <tr>
            <td>
              <div style="display: flex; justify-content: center; align-items: center">
                AUC
                <n-popover trigger="hover">
                  <template #trigger>
                    <n-icon size="20">
                      <Info16Regular />
                    </n-icon>
                  </template>
                  <p>越高分类效果越好，最高为1</p>
                </n-popover>
              </div>
            </td>
            <td colspan="4">
              {{ data[20] }}
            </td>
          </tr>
        </tbody>
      </n-table>
    </n-card>
    <template #description> loading </template>
  </n-spin>
</template>

<style scoped></style>
