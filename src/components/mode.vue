<template>
  <div class="mode">
    <div v-if="mode === 'standard'" class="mode-select">
      <div class="mode-select-pre">当前</div>

      <el-dropdown @command="changeFloor">
        <div class="mode-select-next">
          {{ floor }}号楼

          <el-icon class="mode-select-next-icon">
            <arrow-down />
          </el-icon>
        </div>

        <template #dropdown>
          <el-dropdown-menu>
            <el-dropdown-item v-for="i in count" :key="i" :command="i"
              >{{ i }}号楼</el-dropdown-item
            >
          </el-dropdown-menu>
        </template>
      </el-dropdown>
    </div>
    <div v-if="mode === 'all'" class="mode-box">
      <div class="mode-box-dot"></div>
      <div class="mode-box-text">通讯失败</div>
      <div class="mode-box-dot blue"></div>
      <div class="mode-box-text">&lt;18℃</div>
      <div class="mode-box-dot green"></div>
      <div class="mode-box-text">18℃-24℃</div>
      <div class="mode-box-dot red"></div>
      <div class="mode-box-text">&gt;18℃</div>
    </div>
    <div v-if="mode === 'standard'" class="mode-box">
      <div class="mode-box-dot"></div>
      <div class="mode-box-text">停暖</div>
      <div class="mode-box-dot green"></div>
      <div class="mode-box-text">正常</div>
    </div>
    <div
      v-for="item in modeList"
      :key="item.id"
      class="mode-item"
      :class="{ checked: item.id === current }"
      @click="current = item.id"
    >
      {{ item.name }}
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { ArrowDown } from '@element-plus/icons-vue';

const props = defineProps({
  mode: {
    type: String,
    default: 'all',
  },
  floor: {
    type: Number,
    default: 1,
  },
  count: {
    type: Number,
    default: 1,
  },
});
const emit = defineEmits(['change-floor']);

const current = ref(4);
const modeList = ref([
  {
    id: 1,
    name: '阀位',
  },
  {
    id: 2,
    name: '回温',
  },
  {
    id: 3,
    name: '流量',
  },
  {
    id: 4,
    name: '室温',
  },
  // , {
  //     id: 5,
  //     name: '欠费',
  // }, {
  //     id: 6,
  //     name: '停暖',
  // }
]);

const changeFloor = (v) => {
  emit('change-floor', v);
};
</script>

<style lang="scss" scoped>
.mode {
  position: fixed;
  top: 80px;
  right: 20px;
  display: flex;
  align-items: center;

  &-box {
    display: flex;
    align-items: center;
    padding: 0 6px 0 20px;
    height: 30px;
    background-color: #061339;
    color: #fff;
    border-radius: 5px;

    &-dot {
      width: 14px;
      height: 14px;
      border-radius: 50%;
      background-color: #ccc;

      &.blue {
        background-color: #1962b4;
      }
      &.green {
        background-color: #1f98a4;
      }
      &.red {
        background-color: #a13e19;
      }
    }

    &-text {
      margin: 0 16px 0 10px;
    }
  }

  &-item {
    padding: 0 12px;
    // border: 1px solid #2dbee9;
    // border-radius: 3px;
    // background-color: transparent;
    line-height: 30px;
    margin-left: 16px;
    color: #fff;
    background: url('@/assets/box1.png') no-repeat;
    background-size: 100% 100%;
    cursor: pointer;

    &.checked {
      background: url('@/assets/box2.png') no-repeat;
      background-size: 100% 100%;
    }
  }

  &-select {
    position: fixed;
    top: 80px;
    left: 30px;
    background-color: #13a0ad;
    border-radius: 4px;
    height: 30px;
    line-height: 30px;
    display: flex;
    color: #fff;

    &-pre {
      padding: 0 8px 0 10px;
    }

    &-next {
      padding: 0 10px;
      background-color: #056a74;
      border-top-right-radius: 4px;
      border-bottom-right-radius: 4px;
      color: #fff;
      display: flex;
      align-items: center;

      &-icon {
        margin-left: 8px;
      }
    }
  }
}
</style>
