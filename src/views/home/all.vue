<template>
  <div class="all">
    <div class="all-circle"></div>
    <img class="all-arrow left" :src="arrowRImg" @click="onPrev" />
    <img class="all-arrow right" :src="arrowLImg" @click="onNext" />

    <mode-box />

    <div class="all-house">
        <div class="all-house-title">碧水兰庭 {{ current }}号楼</div>
        <div class="all-house-top">
            <div></div>
            <div></div>
            <div></div>
        </div>
        <div class="all-house-column">
            <div v-for="u in unitNum" :key="u">
                {{ `${u}单元` }}
            </div>
        </div>
        <div class="all-house-body">
            <div class="all-house-unit" v-for="unit in unitNum" :key="unit">
                <div class="all-house-layer" v-for="layer in layerNum" :key="layer">
                    <div class="all-house-floor" v-for="floor in doorNum" :key="floor">
                        <house :width="58" :height="46" :color="unit === 2 && layer === 5 && floor === 3 ? '#a13e19' : unit === 3 && layer === 2 && floor === 1 ? '#1962b4' : unit === 1 && layer === 7 && floor === 2 ? '#ccc' : '#1f98a4'" />
                        <!-- <house :width="58" :height="46" color="#1962b4" /> -->
                        <!-- <house :width="58" :height="46" color="#a13e19" /> -->
                        <div v-if="floor < doorNum" class="all-house-floor-line"></div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <floor-base ref="floorBase" :count="floorNum" @change="onChangeFloor" />

    <div>
        <div class="all-info left">
            <div class="all-info-text left">
                <div>正常供热：</div>
                <div>{{ data.d3 }}</div>
            </div>
        </div>
        <div class="all-info left">
            <div class="all-info-text left">
                <div>停供用户：</div>
                <div>{{ data.d4 }}</div>
            </div>
        </div>
        <div class="all-info left">
            <div class="all-info-text left">
                <div>欠费用户：</div>
                <div>{{ data.d5 }}</div>
            </div>
        </div>
    </div>

    <div>
        <div class="all-info right">
            <div class="all-info-text right">
                <div>18℃以下：</div>
                <div>{{ data.d6 }}</div>
            </div>
        </div>
        <div class="all-info right">
            <div class="all-info-text right">
                <div>18℃-24℃：</div>
                <div>{{ data.d7 }}</div>
            </div>
        </div>
        <div class="all-info right">
            <div class="all-info-text right">
                <div>24℃以上：</div>
                <div>{{ data.d8 }}</div>
            </div>
        </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import ModeBox from '@/components/mode.vue';
import House from '@/components/house.vue';
import FloorBase from '@/components/floor-base.vue';
import arrowLImg from "@/assets/arrow-l.png";
import arrowRImg from "@/assets/arrow-r.png";

const props = defineProps({
    data: {
        type: Object,
        default: () =>({})
    }
})

const current = ref(1);
const floorBase = ref();

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

const onChangeFloor = (v) => {
    current.value = v;
}

const onPrev = () => {
    const v = current.value === 1 ? floorNum.value : current.value - 1;

    floorBase.value.go(v);
}

const onNext = () => {
    const v = current.value === floorNum.value ? 1 : current.value + 1;

    floorBase.value.go(v);
}

</script>

<style lang="scss" scoped>
.all {
  width: 100%;
  height: 100%;
  background-color: #0b2454;
  background-image: url('@/assets/bg.png');
  background-size: 100% 100%;
  position: relative;
  overflow: hidden;
  margin-top: 16px;

  &-circle {
    width: 100%;
    height: 100%;
    background: url('@/assets/circle.png') 50% 56% no-repeat;
    background-size: 80% 80%;
  }

  &-arrow {
    position: absolute;
    top: 50%;
    width: 28px;
    height: 30px;

    &.left {
      left: 12%;
    }
    &.right {
      right: 12%;
    }
  }

  &-info {
    position: absolute;
    top: 35%;
    width: 210px;
    height: 85px;

    &.left {
        background: url('@/assets/icon1.png') center no-repeat;
        background-size: 100% 100%;
        left: 16%;

        &:nth-child(2) {
            left: 14%;
        }

        &:nth-child(3) {
            left: 16%;
        }
    }
    &.right {
        background: url('@/assets/icon2.png') center no-repeat;
        background-size: 100% 100%;
        right: 16%;

        &:nth-child(2) {
            right: 14%;
        }

        &:nth-child(3) {
            right: 16%;
        }
    }

    &:nth-child(2) {
        top: 44%;
    }

    &:nth-child(3) {
        top: 53%;
    }

    &-text {
        display: flex;
        position: absolute;
        top: 35px;
        line-height: 20px;

        &.left {
            left: 80px;
        }
        &.right {
            right: 84px;
        }

        div:nth-child(1) {
            font-size: 13px;
            color: #ccc;
        }
        div:nth-child(2) {
            color: #fff;
        }
    }
    
  }

	&-house {
        position: absolute;

        left: 30%;
        bottom: 230px;
        z-index: 9;

        // display: none;

        &-title {
            text-align: center;
            margin-bottom: 30px;
            font-size: 26px;
            color: #fff;
            text-shadow: 0 0 10px #7ff5eb, 0 0 20px #5ae7d5, 0 0 30px #7ff5eb, 0 0 40px #7ff5eb;
        }

        &-top {
                display: flex;

                div:nth-child(1) {
                    width: 102px;
                    height: 86px;
                    background: url('@/assets/wdl.png') no-repeat;
                }
                div:nth-child(2) {
                    flex: 1;
                    height: 86px;
                    margin: 0 -1px;
                    background: url('@/assets/wdm.png');
                }
                div:nth-child(3) {
                    width: 108px;
                    height: 86px;
                    background: url('@/assets/wdr.png') no-repeat;
                }
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
                background: url('@/assets/unit-bg.png') center no-repeat;
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
}
</style>