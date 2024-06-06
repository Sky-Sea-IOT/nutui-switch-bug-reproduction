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

<style lang="scss" scoped>
.btn {
  @apply: p-4 text-center;
}
</style>

<route lang="json5" type="home">
{
  style: {
    navigationStyle: 'custom',
    navigationBarTitleText: '已安装 nut-ui uniapp',
  },
}
</route>
