# nut-switch 组件 change 事件参数验证失败

## 描述

### 问题分析

**事件触发和警告**：当父组件的数据变化时，子组件的 `switch` 状态也随之变化，但是触发了 `nut-switch` 的 `change` 事件，并且出现了警告：

```
[Vue warn]: Invalid event arguments: event validation failed for event "change".
```

#### 期望的解决方案

当父组件的数据改变时，更新子组件的 `switchItem.value` 但不触发 `change` 事件。

#### 环境

- **Vue 版本**：3.2.47
- **nut-switch 组件版本**：1.7.10

#### 重现代码

**父组件**

```vue
<script lang="ts" setup>
// 父组件
const isSwitched = ref(false);

const switchGroupFalse = reactive({
  sw0: false,
  sw1: false,
  sw2: false,
});

const switchGroupTrue = reactive({
  sw0: true,
  sw1: true,
  sw2: true,
});

const currentSwitchGroup = computed(() => isSwitched.value ? switchGroupTrue : switchGroupFalse);
</script>

<template>
  <view class="btn">
    <nut-button type="primary" @click="isSwitched = !isSwitched">
      点击 {{ isSwitched ? '关闭' : '打开' }}
    </nut-button>
  </view>
  <switch-group :data="currentSwitchGroup" />
</template>
```

**子组件**

```vue
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
    switches.value[0].value = newData.sw0;
    switches.value[1].value = newData.sw1;
    switches.value[2].value = newData.sw2;
}, { deep: true }
);

const change = (switchItem: SwitchItem, value: boolean) => {
    console.log(`Switch ${switchItem.label} value: ${value}`);
    switchItem.value = value;
};
</script>

<template>
    <view class="box">
        <view v-for="switchItem in switches" :key="switchItem.id" class="item">
            <view>{{ switchItem.label }}</view>
            <nut-switch v-model="switchItem.value" @change="(value: any) => change(switchItem, value)" />
        </view>
    </view>
</template>
```
