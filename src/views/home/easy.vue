<template>
  <div class="easy">
    <mode-box
      mode="easy"
      :count="floorNum"
      :floor="floor"
      @change-floor="changeFloor"
    />
    <div class="easy-title">碧水兰庭</div>

    <house-info ref="houseInfo" />

    <div class="easy-arrow left" v-show="carouselList.length > 1" @click="onPrev">
      <img :src="arrowRImg" />
      更多
    </div>

    <div class="easy-arrow right" v-show="carouselList.length > 1" @click="onNext">
      <img :src="arrowLImg" />
      更多
    </div>

    <div class="easy-main">
      <el-carousel
        ref="carousel"
        height="calc(100vh - 170px)"
        :autoplay="false"
        indicator-position="none"
        arrow="never"
      >
        <el-carousel-item
          v-for="(carousel, index) in carouselList"
          :key="index"
        >
          <div v-for="item in carousel" :key="item.name" class="easy-box">
            <div class="easy-box-header">{{ item.name }}</div>
            <div class="easy-box-main">
              <div
                class="easy-box-layer"
                v-for="(layer, lidx) in item.children"
                :key="lidx"
              >
                <div v-for="(house, idx) in layer" :key="idx" class="easy-box-layer-card" :class="getHouseCls(house)" @dblclick="showHouseInfo">
                    {{ house.name }}
                </div>
              </div>
            </div>
            <div class="easy-box-bg">
              <div></div>
              <div></div>
              <div></div>
            </div>
          </div>
        </el-carousel-item>
      </el-carousel>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';
import ModeBox from '@/components/mode.vue';
import arrowLImg from '@/assets/arrowls.png';
import arrowRImg from '@/assets/arrowrs.png';
import HouseInfo from './house-info.vue';
import { standardData } from './standard-data.js';

const props = defineProps({
  data: {
    type: Object,
    default: () => ({}),
  },
  floor: {
    type: Number,
    default: 1,
  },
});
const emit = defineEmits(['change-floor']);

const carousel = ref();
const carouselList = ref([]);
const houseInfo = ref();

// 楼数
const floorNum = computed(() => {
  return props.total ?? 14;
});

const onPrev = () => {
  carousel.value.prev();
};

const onNext = () => {
  carousel.value.next();
};

onMounted(() => {
  getCarouselList();
});

const getCarouselList = () => {
  const w = window.innerWidth - 160;

  let res = [];
  let layer = [];
  let total = 0;

  for (let i = 0; i < standardData.length; i++) {
    const num = standardData[i].num;

    total += 104 * num + 11;

    if (total <= w) {
      layer.push(standardData[i]);
      total += 20;
    } else {
      total = 0;
      res.push(layer);
      layer = [standardData[i]];
    }
  }

  if (layer.length) {
    res.push(layer);
  }

//   console.log(res);
  carouselList.value = res;
};

const getHouseCls = (house) => {
    const t = house.t1;

    return {
        blue: t && t < 16,
        red: t && t > 40,
        grey: !t
    }
}

const changeFloor = (v) => {
  emit('change-floor', v);
};

const showHouseInfo = (e) => {
//   console.log(e)
  houseInfo.value.show({
    x: e.x,
    y: e.y - 60,
    unit: '1601',
    d1: '52%',
    d2: '37.9',
    d3: '60.4'
  });
}
</script>

<style lang="scss" scoped>
.easy {
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
    text-shadow: 0 0 10px #7ff5eb, 0 0 20px #5ae7d5, 0 0 30px #7ff5eb,
      0 0 40px #7ff5eb;
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
    // width: 450px;
    height: calc(100vh - 170px);
    margin-right: 20px;

    &:last-child {
      margin-right: 0;
    }

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

      background-image: -webkit-linear-gradient(top, #fff, #d3faff, #108fa1);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }

    &-main {
      position: relative;
      z-index: 9;
      height: calc(100vh - 245px);
      overflow: auto;
      padding-left: 5px;
      margin-right: 4px;
    }

    &-layer {
      display: flex;

      &-card {
        margin: 0 5px 10px 5px;
        width: 104px;
        height: 37px;
        line-height: 37px;
        background: url('@/assets/house_box1.png');
        text-align: center;
        color: #fff;
        cursor: pointer;

        &.grey {
            background: url('@/assets/house_box2.png');
        }
        &.blue {
            background: url('@/assets/house_box3.png');
        }
        &.red {
            background: url('@/assets/house_box4.png');
        }
      }
    }
  }
}
</style>
