<template>
    <div class="marker" v-show="visible" :style="{ left: `${markerLeft}px`, top: `${markerTop}px` }">
      <div class="marker-close" @click="hide">
        <svg t="1718319645381" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="1453" width="12" height="12"><path d="M562.9 513.3l266-266c14.1-14.1 14.1-36.9 0-50.9-14.1-14.1-36.9-14.1-50.9 0l-266 266-266-266c-14.1-14.1-36.9-14.1-50.9 0-14.1 14.1-14.1 36.9 0 50.9l266 266-265.6 265.6c-14.1 14.1-14.1 36.9 0 50.9 7 7 16.2 10.5 25.5 10.5s18.4-3.5 25.5-10.5L512 564.2l265.6 265.6c7 7 16.2 10.5 25.5 10.5s18.4-3.5 25.5-10.5c14.1-14.1 14.1-36.9 0-50.9L562.9 513.3z" p-id="1454" fill="#ffffff"></path></svg>
      </div>
      <div class="marker-header">单元号: {{ info.unit }}</div>
      <div class="marker-body">
          <div>阀位: {{ info.d1 }}</div>
          <div>回温: {{ info.d2 }}℃</div>
          <div>流量: {{ info.d3 }}t/h</div>
      </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';

const visible = ref(false);
const markerLeft = ref(0);
const markerTop = ref(0);
const info = ref({});

const show = (data = {}) => {
    visible.value = true;
    markerLeft.value = data.x;
    markerTop.value = data.y;
    info.value = data;
}

const hide = () => {
    visible.value = false;
}

defineExpose({
    show
});
</script>

<style lang="scss" scoped>
.marker {
  width: 120px;
  height: 96px;
  position: absolute;
  left: 0;
  top: 0;

  z-index: 99;

  border: 1px solid #999;
  border-radius: 10px;
  background-color: rgba(0, 0, 0, 0.5);
  color: #fff;
  font-size: 12px;

  &-close {
    position: absolute;
    right: 4px;
    top: 4px;
    cursor: pointer;
  }

    &-header {
      padding: 0 10px;
      background-color: #3b414b;
      line-height: 26px;
      border-top-left-radius: 10px;
      border-top-right-radius: 10px;
    }

    &-body {
      padding: 6px 10px;
      line-height: 20px;
    }

    &-arrow {
      position: absolute;
      left: 50%;
      bottom: -1px;
      width: 10px;
      height: 10px;

      &:after {
        content: ' ';
        display: inline-block;
        width: 5px;
        height: 5px;
        transform: rotate(45deg);
        // background-color: #fff;
        background-color: rgba(0, 0, 0, 0.5);
        border-bottom: 1px solid #999;
        border-right: 1px solid #999;
      }
      
    }
}
</style>
  