<script setup>
const props = defineProps({
    load: {
        type: Boolean,
        required: true
    }
})
</script>

<template>
    <n-spin :show="load">
        <n-card title="模型参数" style="height: 300px;">
            哈哈🥰还没做捏
        </n-card>
        <template #description>
            loading
        </template>
    </n-spin>
</template>

<style scoped></style>