<script lang="ts" setup>
// 子组件
const props = defineProps<{
    data: {
        sw0: boolean;
        sw1: boolean;
        sw2: boolean;
    };
}>()

interface SwitchItem {
    id: number;
    label: string;
    value: boolean;
}

const switches = ref<SwitchItem[]>([
    { id: 1, label: '开关1', value: props.data.sw0 },
    { id: 2, label: '开关2', value: props.data.sw1 },
    { id: 3, label: '开关3', value: props.data.sw2 },
]);

watch(() => props.data, (newData) => {
    switches.value = [
        { id: 1, label: '开关1', value: newData.sw0 },
        { id: 2, label: '开关2', value: newData.sw1 },
        { id: 3, label: '开关3', value: newData.sw2 },
    ];
}, { deep: true }
);

const change = (switchItem: SwitchItem, value: boolean, event: Event) => {
    switchItem.value = value;
    console.log(`Switch ${switchItem.label} value: ${value} \nevent: ${JSON.stringify(event)}`);
};
</script>

<template>
    <view class="box">
        <view v-for="switchItem in switches" :key="switchItem.id" class="item">
            <view>{{ switchItem.label }}</view>
            <nut-switch v-model="switchItem.value"
                @change="(value: boolean, event: Event) => change(switchItem, value, event)" />
        </view>
    </view>
</template>

<style lang="scss" scoped>
.box {
    @apply: flex flex-col items-center;
}

.item {
    @apply: flex flex-col items-center gap-2 p-4 m-4 bg-blue-2 w-50% rounded-lg;
}
</style>
