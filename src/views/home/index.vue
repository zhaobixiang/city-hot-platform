<template>
  <div class="home">
    <div class="home-toolbar">
        <div class="home-toolbar-left">
            <div class="home-toolbar-back">
                <img :src="backImg" />
                返回
            </div>
            <div class="home-toolbar-line"></div>
            <div class="home-toolbar-title">楼栋数字孪生</div>

            <div class="home-toolbar-info">
                <img :src="areaImg" />
                <div class="home-toolbar-info-label">热网面积</div>
                <div class="home-toolbar-info-value">24588m²</div>
                <div class="home-toolbar-info-label">供热面积</div>
                <div class="home-toolbar-info-value">10488m²</div>
            </div>

            <div class="home-toolbar-info">
                <img :src="tempImg" />
                <div class="home-toolbar-info-label">正常供热</div>
                <div class="home-toolbar-info-value">165户</div>
                <div class="home-toolbar-info-label small">停供</div>
                <div class="home-toolbar-info-value">22户</div>
                <div class="home-toolbar-info-label small">欠费</div>
                <div class="home-toolbar-info-value">7户</div>
            </div>

            <div class="home-toolbar-info">
                <img :src="tempImg" />
                <div class="home-toolbar-info-label">18℃以下</div>
                <div class="home-toolbar-info-value">7户</div>
                <div class="home-toolbar-info-label">18℃ -24℃</div>
                <div class="home-toolbar-info-value">127户</div>
                <div class="home-toolbar-info-label">24℃以上</div>
                <div class="home-toolbar-info-value">22户</div>
            </div>
        </div>
        <div class="home-toolbar-right">
            <div class="home-toolbar-select">
                环路
            </div>
            <div class="home-toolbar-right-line"></div>
            <div class="home-toolbar-select active">
                总览模式
            </div>
        </div>
    </div>

    <div class="home-mode">
        <div class="home-mode-box">
            <div class="home-mode-box-dot"></div>
            <div class="home-mode-box-text">通讯失败</div>
            <div class="home-mode-box-dot blue"></div>
            <div class="home-mode-box-text">&lt;18℃</div>
            <div class="home-mode-box-dot green"></div>
            <div class="home-mode-box-text">18℃-24℃</div>
            <div class="home-mode-box-dot red"></div>
            <div class="home-mode-box-text">&gt;18℃</div>
        </div>
        <div class="home-mode-item">阀位</div>
        <div class="home-mode-item">回温</div>
        <div class="home-mode-item">流量</div>
        <div class="home-mode-item">室温</div>
        <div class="home-mode-item">欠费</div>
        <div class="home-mode-item checked">停暖</div>
    </div>

    <div class="home-circle"></div>

    <div class="home-title">碧水兰庭 1号楼</div>

    <div class="home-floor1">
        <!-- <img :src="floorImg" />
        <img :src="floorImg" />
        <img :src="floorImg" />
        <img :src="floorImg" />
        <img :src="floorImg" />
        <img :src="floorImg" />
        <img :src="floorImg" />
        <img :src="floorImg" />
        <img :src="floorImg" />
        <img :src="floorImg" /> -->

        <floor-box v-for="item in 10" :key="item" :title="`${item}号楼`" />
    </div>

    <div class="home-house">
        <div class="home-house-top">
            <div></div>
            <div></div>
            <div></div>
        </div>
        <div class="home-house-title"></div>
        <div class="home-house-body">
            <div class="home-house-unit" v-for="unit in unitNum" :key="unit">
                <div class="home-house-layer" v-for="layer in layerNum" :key="layer">
                    <div class="home-house-floor" v-for="floor in floorNum" :key="floor">
                        <house :width="58" :height="46" :color="unit === 2 && layer === 5 && floor === 3 ? '#a13e19' : unit === 3 && layer === 2 && floor === 1 ? '#1962b4' : unit === 1 && layer === 7 && floor === 2 ? '#ccc' : '#1f98a4'" />
                        <!-- <house :width="58" :height="46" color="#1962b4" /> -->
                        <!-- <house :width="58" :height="46" color="#a13e19" /> -->
                        <div v-if="floor < floorNum" class="home-house-floor-line"></div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <img class="home-arrow-left" :src="arrowRImg" />
    <img class="home-arrow-right" :src="arrowLImg" />

    <div class="home-info">
        <div class="home-info-left">
            <div class="home-info-left-text">
                <div>正常供热：</div>
                <div>165户</div>
            </div>
        </div>
        <div class="home-info-left">
            <div class="home-info-left-text">
                <div>停供用户：</div>
                <div>22户</div>
            </div>
        </div>
        <div class="home-info-left">
            <div class="home-info-left-text">
                <div>欠费用户：</div>
                <div>7户</div>
            </div>
        </div>
    </div>

    <div class="home-info">
        <div class="home-info-right">
            <div class="home-info-right-text">
                <div>18℃以下：</div>
                <div>8户</div>
            </div>
        </div>
        <div class="home-info-right">
            <div class="home-info-right-text">
                <div>18℃-24℃：</div>
                <div>127户</div>
            </div>
        </div>
        <div class="home-info-right">
            <div class="home-info-right-text">
                <div>24℃以上：</div>
                <div>22户</div>
            </div>
        </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import House from '@/components/house.vue';
import FloorBox from '@/components/floor.vue';

import bgImg from "@/assets/bg.png";
import backImg from "@/assets/back.png";
import areaImg from "@/assets/area.png";
import tempImg from "@/assets/temp.png";
import floorImg from "@/assets/floor.png";

import arrowLImg from "@/assets/arrow-l.png";
import arrowRImg from "@/assets/arrow-r.png";
import icon1Img from "@/assets/icon1.png";
import icon2Img from "@/assets/icon2.png";

// 楼层数
const layerNum = ref(9);
// 单元数
const unitNum = ref(3);
// 一个单元的户数
const floorNum = ref(4);

</script>

<style lang="scss" scoped>
.home {
//   width: 100vw;
//   height: 100vh;
  width: 100%;
  height: 100%;
  background-color: #0b2454;
  background-image: url('@/assets/bg.png');
//   background-image: url("@/assets/3.png");
  background-size: 100% 100%;
  position: relative;
  overflow: hidden;

  &-toolbar {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 20px;
    height: 60px;
    background-color: #04225f;
    box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
    color: #fff;
    font-size: 16px;

    &-left {
        display: flex;
        align-items: center;
    }

    &-back {
        display: flex;
        align-items: center;

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
        margin: 0 20px
    }

    &-title {
        font-size: 28px;
        line-height: 60px;
        margin-right: 30px;
    }

    &-info {
        display: flex;
        align-items: center;
        font-size: 12px;

        img {
            width: 35px;
            height: 27px;
            margin-right: 10px;
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
            margin: 0 20px
        }
    }

    &-select {
        padding: 0 12px;
        border: 1px solid #ccc;
        background-color: #dddddd52;
        border-radius: 12px;
        line-height: 24px;

        &.active {
            border-color: #dff1ff;
            background-color: #0f98e0;
        }
    }
  }

  &-mode {
    position: absolute;
    top: 80px;
    right: 20px;
    display: flex;
    align-items: center;

    &-box {
        display: flex;
        align-items: center;
        padding-left: 20px;
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
  }

  &-circle {
    width: 100%;
    height: 100%;
    background: url('@/assets/circle.png') 50% 40% no-repeat;
    background-size: 80% 80%;
  }

  &-title {
    position: absolute;
    top: 150px;
    left: 45%;
    font-size: 26px;
    color: #fff;
    text-shadow: 0 0 10px #7ff5eb, 0 0 20px #5ae7d5, 0 0 30px #7ff5eb, 0 0 40px #7ff5eb;
  }

  &-floor {
    position: absolute;
    bottom: 50px;
    left: calc(50% - 50px);
    transform-style: preserve-3d;

    width: 129px;
    height: 122px;
    // transform: rotateX(10deg);

    img {
        position: absolute;
        width: 129px;
        height: 122px;
        top: 0;
        left: 0;
        transform: translateZ(500px);

        // &:nth-child(1) {
        //     transform: translateX(0) translateY(0);
        // }
        // &:nth-child(2) {
        //     transform: translateX(700px) translateY(-160px);
        // }
        // &:nth-child(3) {
        //     transform: translateX(0) translateY(-320px);
        // }
        // &:nth-child(4) {
        //     transform: translateX(-700px) translateY(-160px);
        // }

        // 单边X轴700px，Y轴 160px

        &:nth-child(1) {
            transform: translateX(0) translateY(0);
        }
        &:nth-child(2) {
            transform: translateX(280px) translateY(-64px);
        }
        &:nth-child(3) {
            transform: translateX(560px) translateY(-128px);
        }
        &:nth-child(4) {
            transform: translateX(560px) translateY(-192px);
        }
        &:nth-child(5) {
            transform: translateX(280px) translateY(-256px);
        }
        &:nth-child(6) {
            transform: translateX(0) translateY(-320px);
        }
        &:nth-child(7) {
            transform: translateX(-280px) translateY(-256px);
        }
        &:nth-child(8) {
            transform: translateX(-560px) translateY(-192px);
        }
        &:nth-child(9) {
            transform: translateX(-560px) translateY(-128px);
        }
        &:nth-child(10) {
            transform: translateX(-280px) translateY(-64px);
        }
    }
  }

  &-floor1 {
    position: absolute;
    bottom: 500px;
    left: calc(50% - 50px);
    // transform-style: preserve-3d;

    width: 129px;
    height: 122px;

    :deep(.floor) {
        &:nth-child(1) {
            transform: rotateY(30deg) translateZ(300px);
        }
    }
  }

  &-house {
    position: absolute;

    left: 30%;
    bottom: 230px;

    &-top {
        border: 5px solid #000;
        border-bottom: none;
        // clip-path: polygon(35px 0, 20px 20px, 100% 0, 100% 0);
        clip-path: polygon(20% 0%, 80% 0%, 100% 100%, 0% 100%);

        div:nth-child(1) {
            height: 10px;
            background-color: #ccc;
        }
        div:nth-child(2) {
            height: 10px;
            background-color: #666;
        }
        div:nth-child(3) {
            height: 10px;
            background-color: #999;
            
            display: flex;

            .line {
                width: 3px;
                height: 100%;
                background-color: #000;
            }
        }
    }

    
    &-title {
        background-color: #ccc;
        border: 5px solid #000;
        height: 40px;
    }

    &-body {
        display: flex;
        background-color: #04225f;
    }

    &-unit {
        // display: flex;
        border-left: 5px solid #000;
        border-right: 5px solid #000;

        margin-right: 10px;

        &:last-child {
            margin-right: 0;
        }
    }

    &-layer {
        display: flex;
        
        border-bottom: 5px solid #000;
    }

    &-floor {
        display: flex;

        &-line {
            width: 5px;
            height: 100%;
            background-color: #000;
        }
    }
  }

  &-arrow {
    &-left {
        position: absolute;
        left: 12%;
        top: 50%;
        width: 28px;
        height: 30px;
    }
    &-right {
        position: absolute;
        right: 12%;
        top: 50%;
        width: 28px;
        height: 30px;
    }
  }

  &-info-left {
    position: absolute;
    left: 16%;
    top: 35%;
    width: 210px;
    height: 85px;
    background: url('@/assets/icon1.png') center no-repeat;
    background-size: 100% 100%;

    &:nth-child(2) {
        top: 44%;
        left: 14%;
    }

    &:nth-child(3) {
        top: 53%;
        left: 16%;
    }

    &-text {
        display: flex;
        position: absolute;
        top: 35px;
        left: 80px;
        line-height: 20px;

        div:nth-child(1) {
            font-size: 13px;
            color: #ccc;
        }
        div:nth-child(2) {
            color: #fff;
        }
    }
    
  }

  &-info-right {
    position: absolute;
    right: 16%;
    top: 35%;
    width: 210px;
    height: 85px;
    background: url('@/assets/icon2.png') center no-repeat;
    background-size: 100% 100%;

    &:nth-child(2) {
        top: 44%;
        right: 14%;
    }

    &:nth-child(3) {
        top: 53%;
        right: 16%;
    }

    &-text {
        display: flex;
        position: absolute;
        top: 35px;
        right: 84px;
        line-height: 20px;

        div:nth-child(1) {
            font-size: 13px;
            color: #ccc;
        }
        div:nth-child(2) {
            color: #fff;
        }
    }
    
  }
}
</style>
