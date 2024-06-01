<template>
    <div class="floor-base">
        <floor class="floor-base-item" v-for="v in count" :key="v" :title="`${v}号楼`" :style="floorStyleList[v - 1]" />
    </div>
</template>

<script setup>
import { computed } from 'vue';
import Floor from './floor.vue';

const props = defineProps({
    count: {
        type: Number,
        default: 10
    }
});

const floorStyleList = computed(() => {
    const count = props.count;
    const mid = count / 2;

    const w = 1400 / mid;
    const h = 320 / mid;

    let res = [];
    let list = [];
    let t = 1;

    for (let i = 0; i < count; i++) {
        let x, y;

        x = w * i;
        y = -190 * i;

        if (x > 700) {
            x = w * (i - t);
            t += 2;
        }

        if (i >= mid) {
            x = -list[i - mid];
            y = -190 * (2 * mid - i);
        }

        list[i] = x;

        res[i] = {
            transform: `translateX(${x}px) translateY(${y}px)`
        };
    }

    console.log(res)

    return res;
});
</script>

<style lang="scss" scoped>
.floor-base {
    position: absolute;
    bottom: 50px;
    left: calc(50% - 50px);
    transform-style: preserve-3d;

    width: 129px;
    height: 122px;

    // &-item {
    //     &:nth-child(1) {
    //         transform: translateX(0) translateY(0);
    //     }
    // }
}
</style>