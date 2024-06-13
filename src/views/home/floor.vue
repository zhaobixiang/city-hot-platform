<template>
  <div class="home">
    <div class="home-toolbar">
      <div class="home-toolbar-left">
        <div class="home-toolbar-back" @click="emit('hide')">
          <img :src="backImg" />
          返回
        </div>
        <div class="home-toolbar-line"></div>
        <div class="home-toolbar-title">
          {{ mode === 'all' ? '楼栋数字孪生' : '楼栋信息' }}
        </div>

        <div class="home-toolbar-info">
          <div class="home-toolbar-info-label">热网面积</div>
          <div class="home-toolbar-info-value">{{ floorData.d1 }}</div>
          <div class="home-toolbar-info-label">供热面积</div>
          <div class="home-toolbar-info-value">{{ floorData.d2 }}</div>
        </div>

        <div class="home-toolbar-info user">
          <div class="home-toolbar-info-label">正常供热</div>
          <div class="home-toolbar-info-value">{{ floorData.d3 }}</div>
          <div class="home-toolbar-info-label small">停供</div>
          <div class="home-toolbar-info-value">{{ floorData.d4 }}</div>
          <div class="home-toolbar-info-label small">欠费</div>
          <div class="home-toolbar-info-value">{{ floorData.d5 }}</div>
        </div>

        <div class="home-toolbar-info temp">
          <div class="home-toolbar-info-label">18℃以下</div>
          <div class="home-toolbar-info-value">{{ floorData.d6 }}</div>
          <div class="home-toolbar-info-label">18℃ -24℃</div>
          <div class="home-toolbar-info-value">{{ floorData.d7 }}</div>
          <div class="home-toolbar-info-label">24℃以上</div>
          <div class="home-toolbar-info-value">{{ floorData.d8 }}</div>
        </div>
      </div>
      <div class="home-toolbar-right">
        <el-dropdown>
          <div class="home-toolbar-select">
            环路

            <el-icon class="home-toolbar-select-icon">
              <arrow-down />
            </el-icon>
          </div>

          <template #dropdown>
            <el-dropdown-menu>
              <el-dropdown-item>环路</el-dropdown-item>
            </el-dropdown-menu>
          </template>
        </el-dropdown>

        <div class="home-toolbar-right-line"></div>

        <el-dropdown @command="changeMode">
          <div class="home-toolbar-select active">
            {{ mode === 'all' ? '总览模式' : '标准模式' }}

            <el-icon class="home-toolbar-select-icon">
              <arrow-down />
            </el-icon>
          </div>

          <template #dropdown>
            <el-dropdown-menu>
              <el-dropdown-item command="all">总览模式</el-dropdown-item>
              <el-dropdown-item command="standard">标准模式</el-dropdown-item>
            </el-dropdown-menu>
          </template>
        </el-dropdown>
      </div>
    </div>

    <all
      v-show="mode === 'all'"
      :floor="floor"
      :data="floorData"
      @change-floor="changeFloor"
    />
    <standard
      v-show="mode === 'standard'"
      :floor="floor"
      :data="floorData"
      @change-floor="changeFloor"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { ArrowDown } from '@element-plus/icons-vue';
import backImg from '@/assets/back.png';
import All from './all.vue';
import Standard from './standard.vue';

const emit = defineEmits(['hide']);

const mode = ref('all');
const floor = ref(1);

const floorData = ref({
  d1: '24588m²',
  d2: '10488m²',
  d3: '165户',
  d4: '22户',
  d5: '7户',
  d6: '7户',
  d7: '127户',
  d8: '22户',
  total: 14, // 楼数量
  layer: 10, // 层数
  unit: 3, // 单元数
  door: 4, // 户数
});

// const floorData = ref({
//   d1: '24588m²',
//   d2: '10488m²',
//   d3: '165户',
//   d4: '22户',
//   d5: '7户',
//   d6: '7户',
//   d7: '127户',
//   d8: '22户',
//   total: 14, // 楼数量
//   layer: 20, // 层数
//   unit: 4, // 单元数
//   door: 4, // 户数
// });

const changeMode = () => {
  mode.value = mode.value === 'all' ? 'standard' : 'all';
};

const changeFloor = (v) => {
  floor.value = v;
};
</script>

<style lang="scss" scoped>
.home {
  width: 100%;
  height: 100%;
  position: relative;
  overflow: hidden;

  &-toolbar {
    position: fixed;
    top: 0;
    left: 0;
    width: calc(100% - 40px);
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 20px;
    height: 60px;
    background-color: #04225f;
    box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
    color: #fff;
    font-size: 16px;
    z-index: 9;

    &-left {
      display: flex;
      align-items: center;
    }

    &-back {
      display: flex;
      align-items: center;
      font-size: 1rem;
      cursor: pointer;

      img {
        width: 19px;
        height: 17px;
        margin-right: 10px;
      }
    }

    &-line {
      width: 1px;
      height: 24px;
      background: #111;
      margin: 0 1rem;
    }

    &-title {
      font-size: 1.5rem;
      margin-right: 30px;
    }

    &-info {
      display: flex;
      align-items: center;
      font-size: 12px;
      padding-left: 50px;
      height: 35px;
      background: url('@/assets/area.png') no-repeat;

      &.user {
        background: url('@/assets/user.png') no-repeat;
      }
      &.temp {
        background: url('@/assets/temp.png') no-repeat;
      }

      &-label {
        width: 30px;
        margin-right: 5px;

        &.small {
          width: 12px;
          margin-right: 10px;
        }

        &.large {
          width: 35px;
        }
      }

      &-value {
        font-size: 16px;
        margin-right: 16px;
        font-weight: 600;
      }
    }

    &-right {
      display: flex;
      align-items: center;

      &-line {
        width: 1px;
        height: 28px;
        background: #1b66ff;
        margin: 0 20px;
      }
    }

    &-select {
      padding: 0 12px;
      border: 1px solid #ccc;
      background-color: #dddddd52;
      border-radius: 12px;
      line-height: 24px;
      color: #fff;
      display: flex;
      align-items: center;

      &.active {
        border-color: #dff1ff;
        background-color: #0f98e0;
      }

      &-icon {
        margin-left: 5px;
      }
    }
  }
}
</style>
