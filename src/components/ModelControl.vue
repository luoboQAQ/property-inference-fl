<script setup>
import { ref } from 'vue'
import data from '@/static/data.json'

const emit = defineEmits(['update'])

function getLabel(value) {
    for (let index = 0; index < options.length; index++) {
        const element = options[index];
        if (element.value === value) {
            return element.label
        }
        for (let i = 0; i < element.children.length; i++) {
            const child = element.children[i];
            if (child.value === value) {
                return child.label
            }
        }
    }
}

function changeModel() {
    if(!task.value || !attrs.value){
        showModal_403.value = true
        message_403.value = "参数空着可不好哦~"
        return
    }
    let find = false
    const attr = attrs.value.split('-')[0]
    const property = Number(attrs.value.split('-')[1])
    // console.log(task.value,attr, property)
    if(attr == task.value){
        showModal_403.value = true
        message_403.value = "训练任务和推理属性不能选一样的哦~"
        return
    }
    for (let index = 0; index < data.length; index++) {
        const element = data[index];
        if (element[0] === task.value && element[1] === attr && element[2] === property) {
            find = true
            break
        }
    }
    if (!find) {
        showModal.value = true
        return
    }
    const tittle = `在${getLabel(task.value)}分类模型上推断${getLabel(attrs.value)}的准确率`
    emit('update', task.value, attr, property, tittle)
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
        label: '眼镜', value: 'glasses', children: [
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

const showModal = ref(false)
const showModal_403 = ref(false)
const message_403 = ref('')
</script>

<template>
    <n-card title="攻击参数配置">
        <div style="display: flex; justify-content: space-between;">
            <n-form-item label="主要训练任务" style="flex-grow:1; margin-right: 30px;">
                <n-select v-model:value="task" :options="[
                    { label: '性别', value: 'gender' },
                    { label: '笑脸', value: 'smile' },
                    { label: '种族', value: 'race' },
                    { label: '眼镜', value: 'glasses' },
                    { label: '年龄', value: 'age' },
                    { label: '发色', value: 'hair' }
                ]" />
            </n-form-item>
            <n-form-item label="推理属性" style="flex-grow:1;">
                <n-cascader v-model:value="attrs" check-strategy="child" :options="options" />
            </n-form-item>
        </div>
        <br><br>
        <div style="display: flex; justify-content: flex-end;">
            <n-button @click="changeModel()" type="primary">开始攻击</n-button>
        </div>
    </n-card>
    <n-modal v-model:show="showModal">
        <n-card style="width: 600px;">
            <n-result status="404" title="攻击模型中" description="数据还未准备好，待会再来吧~">
                <template #footer>
                    <n-button @click="showModal = false">换个参数吧</n-button>
                </template>
            </n-result>
        </n-card>
    </n-modal>
    <n-modal v-model:show="showModal_403">
        <n-card style="width: 600px;">
            <n-result status="403" title="参数异常" :description="message_403">
                <template #footer>
                    <n-button @click="showModal_403 = false">我知道啦</n-button>
                </template>
            </n-result>
        </n-card>
    </n-modal>
</template>

<style scoped></style>