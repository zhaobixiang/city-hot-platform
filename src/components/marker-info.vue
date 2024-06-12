<template>
    <div class="marker" v-show="visible" :style="{ top: `${top}px`, left: `${left}px` }">
        <div class="marker-close" @click="hide"></div>
        <div class="marker-title">2103信息</div>

        <div class="marker-box first">
            <div class="marker-box-left"></div>
            <div class="marker-box-right">
                <div class="marker-box-right-item">
                    <div class="marker-box-right-item-label">阀门开度:</div>
                    <div class="marker-box-right-item-value">52%</div>
                </div>
                <div class="marker-box-right-item">
                    <div class="marker-box-right-item-label">平均回温:</div>
                    <div class="marker-box-right-item-value">34.0℃</div>
                </div>
                <div class="marker-box-right-item">
                    <div class="marker-box-right-item-label">平均室温:</div>
                    <div class="marker-box-right-item-value">36.7℃</div>
                </div>
            </div>
        </div>

        <div class="marker-box">
            <div class="marker-box-left"></div>
            <div class="marker-box-right">
                <div class="marker-box-right-item">
                    <div class="marker-box-right-item-label">供水温度:</div>
                    <div class="marker-box-right-item-value">50.8℃</div>
                </div>
                <div class="marker-box-right-item">
                    <div class="marker-box-right-item-label">瞬时流量:</div>
                    <div class="marker-box-right-item-value">60.4t/h</div>
                </div>
                <div class="marker-box-right-item">
                    <div class="marker-box-right-item-label">瞬时热量:</div>
                    <div class="marker-box-right-item-value">1.80Gj/h</div>
                </div>
                <div class="marker-box-right-item">
                    <div class="marker-box-right-item-label">累计流量:</div>
                    <div class="marker-box-right-item-value">18990.0t</div>
                </div>
                <div class="marker-box-right-item">
                    <div class="marker-box-right-item-label">累计热量:</div>
                    <div class="marker-box-right-item-value">28990.0Gj</div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';

const visible = ref(false);
const top = ref(0);
const left = ref(0);

const show = (data = {}) => {
    // console.log(data);

    top.value = data.y - 278;
    left.value = data.x - 158;
    visible.value = true;
}

const hide = () => {
    visible.value = false;
}

defineExpose({
    show
});
</script>

<style lang="scss" scoped>
.marker {
    position: absolute;
    top: 0;
    left: 0;
    width: 316px;
    height: 278px;
    background: url('@/assets/dialog1.png') no-repeat;

    &-close {
        position: absolute;
        top: 20px;
        right: 12px;
        width: 24px;
        height: 24px;
        background: url('@/assets/dialog-close.png') no-repeat;
        cursor: pointer;
    }

    &-title {
        padding-top: 12px;
        height: 38px;
        line-height: 38px;
        color: #fff;
        font-size: 20px;
        text-align: center;
    }

    &-box {
        display: flex;
        align-items: center;
        margin-top: 12px;
        margin-left: 12px;

        &.first {
            margin-bottom: 24px;
        }

        &-left {
            width: 100px;
            height: 60px;
            flex-shrink: 0;
            background: url('@/assets/marker1.png') center no-repeat;
        }

        &-right {
            flex: 1;

            &-item {
                display: flex;
                align-items: center;
                font-size: 12px;
                height: 20px;
                line-height: 20px;

                &-label {
                    color: #6e6e6e;
                }
                &-value {
                    margin-left: 6px;
                    color: #fc8727;
                }
            }
        }
    }
}
</style>