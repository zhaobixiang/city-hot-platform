<template>
  <div class="standard">
    <mode-box mode="standard" :count="floorNum" :floor="floor" @change-floor="changeFloor" />
    <div class="standard-title">碧水兰庭</div>

    <div class="standard-arrow left" @click="onPrev">
        <img :src="arrowRImg" />
        更多
    </div>

    <div class="standard-arrow right" @click="onNext">
        <img :src="arrowLImg" />
        更多
    </div>

    <div class="standard-main">
        <el-carousel ref="carousel" height="calc(100vh - 170px)" :autoplay="false" indicator-position="none" arrow="never">
            <el-carousel-item v-for="item in 3" :key="item">
                <div class="standard-box" v-for="i in 3" :key="i"> 
                    <div class="standard-box-header">
                        {{ i }}单元
                    </div>
                    <div class="standard-box-main">
                        <house-card class="standard-box-main-card" v-for="v in 10" :key="v" :type="0" @set="onSetting" />
                    </div>
                    <div class="standard-box-bg">
                        <div></div>
                        <div></div>
                        <div></div>
                    </div>
                </div>
            </el-carousel-item>
        </el-carousel>
    </div>
    
    <setting-dialog ref="setting" />
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import ModeBox from '@/components/mode.vue';
import houseCard from '@/components/house-card.vue';
import SettingDialog from '@/components/setting-dialog.vue';

import arrowLImg from "@/assets/arrowls.png";
import arrowRImg from "@/assets/arrowrs.png";

const props = defineProps({
    data: {
        type: Object,
        default: () =>({})
    },
    floor: {
        type: Number,
        default: 1
    }
})
const emit = defineEmits(['change-floor']);

const carousel = ref();
const setting = ref();

// 楼数
const floorNum = computed(() => {
    return props.total ?? 14;
});

const onPrev = () => {
    carousel.value.prev();
}

const onNext = () => {
    carousel.value.next();
}

const changeFloor = (v) => {
    emit('change-floor', v);
}

const onSetting = (v) => {
    setting.value.show(v);
}
</script>

<style lang="scss" scoped>
.standard {
    width: 100%;
    height: 100%;
    margin-top: 60px;
    background-color: #0b2454;
    position: relative;

    &-title {
        padding: 25px 0;
        text-align: center;
        font-size: 26px;
        color: #fff;
        text-shadow: 0 0 10px #7ff5eb, 0 0 20px #5ae7d5, 0 0 30px #7ff5eb, 0 0 40px #7ff5eb;
    }

    &-arrow {
        position: absolute;
        top: 40%;
        width: 20px;
        padding: 24px 5px;
        background-color: #061339;
        color: #fff;
        border-radius: 5px;
        text-align: center;
        cursor: pointer;

        &.left {
            left: 30px;
        }
        &.right {
            right: 30px;
        }

        img {
            width: 15px;
            height: 17px;
            margin-bottom: 10px;
        }
    }

    &-main {
        padding: 0 80px;
        // display: flex;
        // justify-content: center;

        // display: grid;
        // grid-template-columns: repeat(3, 1fr);
        // gap: 20px;
 
        :deep(.el-carousel__item) {
            display: flex;
            justify-content: center;
        }
    }

    &-box {
        position: relative;
        // display: flex;
        // flex-direction: column;
        width: 468px;
        height: calc(100vh - 170px);
        margin-right: 20px;

        &-bg {
            display: flex;
            width: 100%;
            height: 100%;
            position: absolute;
            top: 0;
            left: 0;

            div:nth-child(1) {
                width: 41px;
                height: 100%;
                background: url('@/assets/blockl.png') no-repeat;
                background-size: 100% 100%;
            }
            div:nth-child(2) {
                flex: 1;
                height: 100%;
                background: url('@/assets/blockm.png');
                background-size: 100% 100%;
            }
            div:nth-child(3) {
                width: 41px;
                height: 100%;
                background: url('@/assets/blockr.png') no-repeat;
                background-size: 100% 100%;
            }
        }

        &-header {
            position: relative;
            height: 48px;
            line-height: 48px;
            text-align: center;
            // color: #fff;
            font-size: 16px;
            z-index: 9;

            background-image: -webkit-linear-gradient(top, #fff, #d3faff,#108fa1);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        &-main {
            position: relative;
            z-index: 9;
            display: flex;
            flex-wrap: wrap;
            width: calc(100% - 4px);
            height: calc(100vh - 245px);
            overflow: auto;

            &-card {
                margin: 0 10px 10px 10px;
            }
        }
    }
}
</style>