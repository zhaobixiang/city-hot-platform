<template>
  <div class="house">
    <div class="house-top">
      <div class="house-top-x"></div>
      <div class="house-top-y"></div>
    </div>
    <div class="house-line"></div>
    <div class="house-bottom"></div>
  </div>
</template>

<script setup>
import { computed } from 'vue';

const props = defineProps({
  width: {
    type: [Number, String],
    default: 200,
  },
  height: {
    type: [Number, String],
    default: 158,
  },
  color: {
    type: String,
    default: '#1f98a4',
  },
});

const w = computed(() => {
  return props.width + 'px';
});
const h = computed(() => {
  return props.height + 'px';
});
const bgColor = computed(() => {
  return props.color;
});
</script>

<style lang="scss" scoped>
.house {
  // width: 200px;
  width: v-bind(w);
  height: v-bind(h);

  &-top {
    position: relative;
    background-color: v-bind(bgColor);
    // background-color: #1f98a4;
    // background: linear-gradient(#fff, #5bffff 20%, #00ffff);
    // height: 120px;
    height: 75%;

    &-x {
      position: absolute;
      left: 0;
      // bottom: 20px;
      bottom: 12%;
      width: 100%;
      // height: 8px;
      height: 5%;
      background-color: #000;
    }

    &-y {
      position: absolute;
      top: 0;
      // left: 40px;
      // width: 8px;
      left: 25%;
      width: 4%;
      height: 100%;
      background-color: #000;
    }
  }

  &-line {
    // height: 8px;
    height: 5%;
    background-color: #000;
  }

  &-bottom {
    // height: 30px;
    height: 18%;
    // background-color: aquamarine
    background-color: #8ac2b1;
  }
}
</style>
