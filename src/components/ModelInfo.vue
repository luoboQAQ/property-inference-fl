<script setup>
import { ref } from 'vue';
const props = defineProps({
    load: {
        type: Boolean,
        required: true
    }
})
const trainData = ref(5198);
</script>

<template>
    <n-spin :show="load">
        <n-card title="模型参数" style="height: 300px;">
            <n-grid>
                <n-gi span="12">
                    <n-statistic label="训练集" tabular-nums>
                        <n-number-animation :from="0" :to="trainData" />
                        <template #suffix>
                            组
                        </template>
                    </n-statistic>
                </n-gi>
                <n-gi span="12">
                    <n-statistic label="测试集" tabular-nums>
                        <n-number-animation :from="0" :to="2229" />
                        <template #suffix>
                            组
                        </template>
                    </n-statistic>
                </n-gi>
            </n-grid>
            <n-grid>
                <n-gi span="12">
                    <n-statistic label="攻击者有标签数据" tabular-nums>
                        <n-number-animation :from="0" :to="514" />
                        <template #suffix>
                            组
                        </template>
                    </n-statistic>
                </n-gi>
                <n-gi span="12">
                    <n-statistic label="攻击者无标签数据" tabular-nums>
                        <n-number-animation :from="0" :to="3046" />
                        <template #suffix>
                            组
                        </template>
                    </n-statistic>
                </n-gi>
            </n-grid>
        </n-card>
        <template #description>
            loading
        </template>
    </n-spin>
</template>

<style scoped></style>