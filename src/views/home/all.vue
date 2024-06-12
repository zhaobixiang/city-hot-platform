<template>
  <div class="all">
    <div class="all-circle"></div>
    <img class="all-arrow left" :src="arrowRImg" @click="onPrev" />
    <img class="all-arrow right" :src="arrowLImg" @click="onNext" />

    <mode-box />

    <div class="all-house" :style="{ bottom: `${houseBottom}px` }">
      <div class="all-house-title">碧水兰庭 {{ floor }}号楼</div>
      <!-- <div class="all-house-top" :style="{ height: `${houseTopHeight}px` }">
        <div></div>
        <div></div>
        <div></div>
      </div> -->
      <div class="all-house-top1">
      </div>
      <!-- <div class="all-house-column" :style="{ height: `${houseColumnHeight}px` }">
        <div v-for="u in unitNum" :key="u" :style="{ height: `${houseColumnImgHeight}px`, lineHeight: `${houseColumnImgHeight}px` }">
          {{ `${u}单元` }}
        </div>
      </div> -->
      <div class="all-house-body">
        <div
          class="all-house-unit"
          v-for="(unit, uIndex) in unitNum"
          :key="unit"
        >
          <div
            class="all-house-layer"
            v-for="(layer, lIndex) in layerNum"
            :key="layer"
          >
            <div class="all-house-number" v-if="!uIndex">{{ 9 - lIndex }}F</div>
            <div class="all-house-floor" v-for="floor in doorNum" :key="floor">
              <house
                :width="doorWidth"
                :height="doorHeight"
                :color="
                  unit === 2 && layer === 5 && floor === 3
                    ? '#a13e19'
                    : unit === 3 && layer === 2 && floor === 1
                    ? '#1962b4'
                    : unit === 1 && layer === 7 && floor === 2
                    ? '#ccc'
                    : '#1f98a4'
                "
              />
              <!-- <house :width="58" :height="46" color="#1962b4" /> -->
              <!-- <house :width="58" :height="46" color="#a13e19" /> -->
              <div v-if="floor < doorNum" class="all-house-floor-line"></div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <floor-base
      ref="floorBase"
      :floor="floor"
      :count="floorNum"
      @change-floor="changeFloor"
    />

    <div class="all-tag left" :style="{ top: `${tagTop}%`, left: `${tagLeft}%`, transform: `scale(${tagRate})` }">
        <div class="all-tag-item">
            <div class="all-tag-item-text">
                <div>正常供热：</div>
                <div>{{ data.d3 }}</div>
            </div>
        </div>
        <div class="all-tag-item">
            <div class="all-tag-item-text">
                <div>停供用户：</div>
                <div>{{ data.d4 }}</div>
            </div>
        </div>
        <div class="all-tag-item">
            <div class="all-tag-item-text">
                <div>欠费用户：</div>
                <div>{{ data.d5 }}</div>
            </div>
        </div>
    </div>

    <div class="all-tag right" :style="{ top: `${tagTop}%`, right: `${tagLeft}%`, transform: `scale(${tagRate})` }">
        <div class="all-tag-item">
            <div class="all-tag-item-text">
                <div>正常供热：</div>
                <div>{{ data.d3 }}</div>
            </div>
        </div>
        <div class="all-tag-item">
            <div class="all-tag-item-text">
                <div>停供用户：</div>
                <div>{{ data.d4 }}</div>
            </div>
        </div>
        <div class="all-tag-item">
            <div class="all-tag-item-text">
                <div>欠费用户：</div>
                <div>{{ data.d5 }}</div>
            </div>
        </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onBeforeUnmount } from "vue";
import ModeBox from "@/components/mode.vue";
import House from "@/components/house.vue";
import FloorBase from "@/components/floor-base.vue";
import arrowLImg from "@/assets/arrow-l.png";
import arrowRImg from "@/assets/arrow-r.png";

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
const emit = defineEmits(["change-floor"]);

const floorBase = ref();
const tagTop = ref(35);
const tagRate = ref(1);
const tagLeft = ref(14);
const doorWidth = ref(58);
const doorHeight = ref(46);

const houseTopHeight = ref(86);
const houseColumnHeight = ref(50);
const houseColumnImgHeight = ref(40);
const houseLeft = ref(46);
const houseBottom = ref(230);

// 楼数
const floorNum = computed(() => {
  return props.total ?? 14;
});
// 楼层数
const layerNum = computed(() => {
  return props.layer ?? 9;
});
// 单元数
const unitNum = computed(() => {
  return props.unit ?? 3;
});
// 一个单元的户数
const doorNum = computed(() => {
  return props.door ?? 4;
});

onMounted(() => {
    resize();
    window.addEventListener('resize', resize);
});

onBeforeUnmount(() => {
    window.removeEventListener('resize', resize);
});

const setTagTop = () => {
    const h = window.innerHeight;

    tagTop.value = parseInt(h / 1080 * 35);
};

const setDoorSize = () => {
    const w = window.innerWidth;

    const total = parseInt(w / 1920 * 720);

    doorWidth.value = parseInt((total - 10 * (unitNum.value - 1) - 5 * (doorNum.value + 1) * unitNum.value) / (unitNum.value * doorNum.value));
    doorHeight.value = parseInt(doorWidth.value / 58 * 46);
};

const resize = () => {
    setTagTop();
    setDoorSize();

    const rate = window.innerWidth / 1920;

    houseTopHeight.value = parseInt(rate * 86);
    houseColumnHeight.value = parseInt(rate * 50);
    houseColumnImgHeight.value = parseInt(rate * 40);
    houseBottom.value = Math.round(rate * 230);
    tagRate.value = rate;
    tagLeft.value = 14 - parseInt((1 - rate) * 10);
}

const changeFloor = (v) => {
  emit("change-floor", v);
};

const onPrev = () => {
  const v = props.floor === 1 ? floorNum.value : props.floor - 1;

  changeFloor(v);
  floorBase.value.go(v);
};

const onNext = () => {
  const v = props.floor === floorNum.value ? 1 : props.floor + 1;

  changeFloor(v);
  floorBase.value.go(v);
};
</script>

<style lang="scss" scoped>
.all {
    width: 100%;
    height: 100%;
    background-color: #0b2454;
    background-image: url("@/assets/bg.png");
    background-size: 100% 100%;
    position: relative;
    overflow: hidden;
    margin-top: 16px;

    &-circle {
        width: 100%;
        height: 100%;
        background: url("@/assets/circle.png") 50% 56% no-repeat;
        background-size: 80% 80%;
    }

    &-arrow {
        position: absolute;
        top: 47%;
        width: 28px;
        height: 30px;

        &.left {
            left: 12%;
        }
        &.right {
            right: 12%;
        }
    }

    &-tag {
        position: absolute;
        // top: 35%;
        
        &.left {
            left: 14%;

            .all-tag-item {
                &:nth-child(1),
                &:nth-child(3) {
                    margin-left: 30px;
                }

                &-text {
                    left: 80px;
                }
            }
        }
        &.right {
            right: 14%;

            .all-tag-item {
                background: url("@/assets/icon2.png") center no-repeat;
                background-size: 100% 100%;

                &:nth-child(2) {
                    margin-left: 30px;
                }

                &-text {
                    right: 84px;
                }
            }
        }

        &-item {
            width: 210px;
            height: 85px;
            background: url("@/assets/icon1.png") center no-repeat;
            background-size: 100% 100%;
            position: relative;

            &-text {
                display: flex;
                position: absolute;
                top: 35px;
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

    &-house {
        position: absolute;

        left: 31%;
        bottom: 230px;
        // bottom: 170px;
        z-index: 9;

        // display: none;

        &-title {
            text-align: center;
            margin-bottom: 30px;
            font-size: 26px;
            color: #fff;
            text-shadow: 0 0 10px #7ff5eb, 0 0 20px #5ae7d5, 0 0 30px #7ff5eb,
                0 0 40px #7ff5eb;
        }

        &-top {
            display: flex;
            height: 86px;

            div:nth-child(1) {
                width: 102px;
                // height: 86px;
                background: url("@/assets/wdl.png") no-repeat;
                background-size: 100% 100%;
            }
            div:nth-child(2) {
                flex: 1;
                // height: 86px;
                margin: 0 -1px;
                background: url("@/assets/wdm.png");
                background-size: 100% 100%;
            }
            div:nth-child(3) {
                width: 108px;
                // height: 86px;
                background: url("@/assets/wdr.png") no-repeat;
                background-size: 100% 100%;
            }
        }

        &-top1 {
          width: 500px;
          height: 80px;
          background: url('@/assets/house-top.png') no-repeat;
          background-size: 100% 100%;
        }

        &-column {
            background-color: #c6d3d4;
            border: 5px solid #000;
            height: 50px;
            display: flex;
            align-items: center;
            justify-content: space-around;

        div {
            width: 120px;
            height: 40px;
            line-height: 40px;
            background: url("@/assets/unit-bg.png") center no-repeat;
            background-size: 100% 100%;
            text-align: center;
            color: #fff;
            font-size: 16px;
        }
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
        position: relative;
        border-bottom: 5px solid #000;
    }

    &-number {
        font-size: 14px;
        color: #fff;
        position: absolute;
        bottom: 5px;
        left: -30px;
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
}
</style>