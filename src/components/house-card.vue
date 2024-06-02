<template>
  <div class="house" :class="houseCls">
    <div class="house-header">
      <img :src="icon1Img" />
      <img :src="icon2Img" />
      <img :src="icon3Img" />
      <img :src="icon4Img" />
      <img :src="icon5Img" />
      <div class="house-header-text">策略</div>
      <img :src="icon6Img" />
    </div>
    <div class="house-main">
      <div class="house-main-top">
        <div class="house-main-top-left">
          <div class="house-main-top-left-item">
            <span>室内:</span>
            <span>23℃</span>
          </div>
          <div class="house-main-top-left-item">
            <span>设置:</span>
            <span>24℃</span>
          </div>
        </div>
        <div class="house-main-top-right">
          <div>1601室</div>
          <div>145㎡</div>
          <img :src="houseImg" />
        </div>
      </div>
      <div class="house-main-info">
        <div class="house-main-info-item">
          <div class="house-main-info-item-label">供水温度</div>
          <div class="house-main-info-item-value">45℃</div>
        </div>
        <div class="house-main-info-item">
          <div class="house-main-info-item-label">瞬时流量</div>
          <div class="house-main-info-item-value">60.4t/h</div>
        </div>
        <div class="house-main-info-item">
          <div class="house-main-info-item-label">回水温度</div>
          <div class="house-main-info-item-value">45℃</div>
        </div>

      </div>
      <div class="house-main-bottom">
        <div class="house-main-bottom-text">阀门开度  86%</div>
        <img :src="progressImg" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue';
import icon1Img from "@/assets/c-icon1.png";
import icon2Img from "@/assets/c-icon2.png";
import icon3Img from "@/assets/c-icon3.png";
import icon4Img from "@/assets/c-icon4.png";
import icon5Img from "@/assets/c-icon5.png";
import icon6Img from "@/assets/c-icon6.png";

import house1Img from "@/assets/house1.png";
import house2Img from "@/assets/house2.png";
import house3Img from "@/assets/house3.png";
import house4Img from "@/assets/house4.png";

import progress1Img from "@/assets/progress1.png";
import progress2Img from "@/assets/progress2.png";
import progress3Img from "@/assets/progress3.png";
import progress4Img from "@/assets/progress4.png";

const props = defineProps({
  type: {
    type: Number,
    default: 0
  }
})

const houseCls = computed(() => {
  return ['', 'grey', 'blue', 'red'][props.type];
});

const houseImg = computed(() => {
  return [house1Img, house2Img, house3Img, house4Img][props.type];
});

const progressImg = computed(() => {
  return [progress1Img, progress2Img, progress3Img, progress4Img][props.type];
});

</script>

<style lang="scss" scoped>
.house {
  width: 210px;
  height: 217px;
  background: url('@/assets/card1.png') no-repeat;
  background-size: 100% 100%;
  display: flex;
  flex-direction: column;

  &.grey {
    background: url('@/assets/card2.png') no-repeat;
  }
  &.blue {
    background: url('@/assets/card3.png') no-repeat;
  }
  &.red {
    background: url('@/assets/card4.png') no-repeat;
  }

  &-header {
    height: 40px;
    padding: 5px 5px 0;
    display: flex;
    align-items: center;
    justify-content: space-between;

    &-text {
      font-size: 12px;
      color: #fff;
      border: 1px solid #ccc;
      border-radius: 8px;
      height: 16px;
      line-height: 16px;
      padding: 2px;
      margin: 0 4px;
    }
    
    img {
      width: 28px;
      height: 22px;

      &:last-child {
        width: 16px;
        height: 19px;
        margin-right: 2px;
      }
    }
  }

  &-main {
    flex: 1;

    &-top {
      display: flex;
      height: 80px;

      &-left {
        flex: 1;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;

        &-item {
          span {
            color: #999;

            &:nth-child(2) {
              color: #fff;
              margin-left: 2px;
              // font-weight: 600;
            }
          }
        }
      }
      
      &-right {
        flex: 1;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        // background: url('@/assets/house1.png') center no-repeat;
        // background-size: 90% 90%;
        
        div {
          font-size: 12px;
          color: #fff;

          &:nth-child(1) {
            font-size: 18px;
            font-weight: 600;
          }
        }
        
        img {
          position: absolute;
          width: 75px;
          height: 80px;
        }
      }
    }

    &-info {
      display: flex;
      margin-top: 2px;

      &-item {
        flex: 1;
        text-align: center;

        &-label {
          font-size: 12px;
          color: #999;
        }
        &-value {
          color: #fff;
        }
      }
    }

    &-bottom {
      position: relative;
      color: #fff;
      display: flex;
      width: 180px;
      height: 16px;
      margin: 16px 0 0 14px;
      font-size: 12px;
      background-color: #5e6161;
      border-radius: 10px;

      &-text {
        position: absolute;
        left: 40px;
        top: 0;
        line-height: 15px;
      }

      img {
        width: 180px;
        height: 16px;
      }
    }
  }
}
</style>
