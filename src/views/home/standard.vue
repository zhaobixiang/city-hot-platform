<template>
  <div class="standard">
    <mode-box mode="standard" />
    <div class="standard-title">碧水兰庭</div>

    <div class="standard-arrow left">
        <img :src="arrowRImg" />
        更多
    </div>

    <div class="standard-arrow right">
        <img :src="arrowLImg" />
        更多
    </div>

    <div class="standard-main">
        <div class="standard-box" v-for="i in 5" :key="i"> 
            <div class="standard-box-header">
                1单元
            </div>
            <div class="standard-box-main">
                <house-card class="standard-box-main-card" v-for="v in 10" :key="v" :type="0" />
            </div>
            <div class="standard-box-bg">
                <div></div>
                <div></div>
                <div></div>
            </div>
        </div>
    </div>
    
  </div>
</template>

<script setup>
import { ref } from 'vue';
import ModeBox from '@/components/mode.vue';
import houseCard from '@/components/house-card.vue';

import arrowLImg from "@/assets/arrowls.png";
import arrowRImg from "@/assets/arrowrs.png";

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
        display: flex;
        justify-content: center;
        // display: grid;
        // grid-template-columns: repeat(3, 1fr);
        // gap: 20px;
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
            color: #fff;
            font-size: 16px;
            z-index: 9;
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