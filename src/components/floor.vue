<template>
  <div class="floor">
    <div class="floor-title">{{ title }}</div>
    <img :src="active ? floorActiveImg : floorImg" :style="{ width: `${width}px`, height: `${height}px` }" />
  </div>
</template>

<script setup>
import floorImg from "@/assets/floor.png";
import floorActiveImg from "@/assets/floor-active.png";

defineProps({
  title: {
    type: String,
    default: '1号楼'
  },
  width: {
    type: [String, Number],
    default: 129
  },
  height: {
    type: [String, Number],
    default: 122
  },
  active: {
    type: Boolean,
    default: false
  }
});
</script>

<style lang="scss" scoped>
.floor {
    display: flex;
    flex-direction: column;
    align-items: center;

    &-title {
        font-size: 14px;
        color: #fff;
        font-weight: 600;
        margin-bottom: 8px;
    }

    img {
        width: 129px;
        height: 122px;
    }
}
</style>