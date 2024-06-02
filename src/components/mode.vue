<template>
  <div class="mode">
    <div v-if="mode === 'standard'" class="mode-select">
        <div class="mode-select-pre">当前</div>
        <div class="mode-select-next">
            一号楼
        </div>
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
    <div class="mode-item" v-for="item in modeList" :key="item.id" :class="{ checked: item.id === current }">
        {{ item.name }}
    </div>
</div>
</template>

<script setup>
import { ref } from 'vue';

const props = defineProps({
    mode: {
        type: String,
        default: 'all'
    }
});

const current = ref(6);
const modeList = ref([{
    id: 1,
    name: '阀位',
}, {
    id: 2,
    name: '回温',
}, {
    id: 3,
    name: '流量',
}, {
    id: 4,
    name: '室温',
}, {
    id: 5,
    name: '欠费',
}, {
    id: 6,
    name: '停暖',
}]);

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
        border: 1px solid #2dbee9;
        border-radius: 3px;
        background-color: transparent;
        line-height: 30px;
        margin-left: 20px;
        color: #fff;

        &.checked {
            background-color: #13a0ad;
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
        }
    }
  }
</style>