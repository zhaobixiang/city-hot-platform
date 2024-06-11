<template>
    <div class="floor-base" :style="{ width: `${imgWidth}px`, height: `${imgHeight}px`, left: `calc(50% - ${rootLeft}px)`, bottom: `${rootBottom}px`, transform: `rotateX(${xDeg}deg)` }">
        <floor class="floor-base-item" v-for="v in count" :key="v" :title="`${v}号楼`" :width="imgWidth" :height="imgHeight" :active="current === v" :style="styleList[v - 1]" @click="onClick(v)" />
    </div>
</template>

<script setup>
import { ref, computed, watch, onMounted, onBeforeUnmount } from 'vue';
import Floor from './floor.vue';

const props = defineProps({
    count: {
        type: Number,
        default: 10
    },
    floor: {
        type: Number,
        default: 1
    }
});

const emit = defineEmits(['change-floor']);

const current = ref(props.floor);
const radius = ref(800);
const degList = ref([]);
const imgWidth = ref(129);
const imgHeight = ref(122);
const rootLeft = ref(50);
const rootBottom = ref(200);
const xDeg = ref(-10);

const styleList = computed(() => {
    return degList.value.map((v, index) => {
        const active = current.value - 1 === index;
        const tz = active ? radius.value - 80 : radius.value;

        return {
            transform: `rotateY(${v}deg) translateZ(${tz}px) rotateY(${-v}deg) ${active ? 'scale(1.3)' : ''}`,
            zIndex: active ? 10 : 1 
        }
    });
});

onMounted(() => {
    resize();

    window.addEventListener('resize', resize);
});

onBeforeUnmount(() => {
    window.removeEventListener('resize', resize);
});

const setDegList = (v) => {
    const num = v ?? props.count;
    const deg = 360 / num;

    let res = [];

    for (let i = 0; i < num; i++) {
        res[i] = Math.round(i * deg);
    }

    degList.value = res;
}

const setRadius = () => {
    const rate = window.innerWidth / 1920;

    radius.value = parseInt(1600 * rate / 2);
}

const setSize = () => {
  const width = window.innerWidth;
  const rate = window.innerWidth / 1920;

  imgWidth.value = 129 * rate;
  imgHeight.value = 122 * rate;
  rootLeft.value = parseInt(50 / 130 * imgWidth.value);
//   rootBottom.value = parseInt(rate * 200 * (window.innerWidth < 1600 ? 1.08 : 1));
  rootBottom.value = width >= 1920 ? 200 : width >= 1680 ? 180 : 160;
//   xDeg.value = rate * -10;
}

const resize = () => {
    setRadius();
    setSize();
};

const onClick = (v, prev) => {
    const last = prev ?? props.floor;
    const num = props.count;
    const deg = 360 / num;
    const size = v >= last ? v - last : (num + v - last);
    const list = degList.value;
    const len = list.length;
    let res = [];

    for (let i = 0; i < len; i++) {
        if (size > num / 2) {
            res[i] = list[i] + Math.round(deg * (num - size));
        } else {
            res[i] = list[i] - Math.round(deg * size);
        }

        // 修正正前方角度
        if (i === v - 1) {
            const d = Math.round(Math.abs(res[i]) / 360);

            res[i] = 360 * d * (res[i] > 0 ? 1 : -1);
        }
    }

    current.value = v;
    degList.value = res;

    emit('change-floor', v);
}

watch(() => props.floor, (v, ov) => {
    if (current.value !== v) {
        onClick(v, ov);
    }
});

watch(() => props.count, (v) => {
    setDegList(v);
}, {
    immediate: true
});

defineExpose({
    go: onClick
});
</script>

<style lang="scss" scoped>
.floor-base {
    position: absolute;
    bottom: 200px;
    left: calc(50% - 50px);
    width: 129px;
    height: 122px;
    transform: rotateX(-10deg);
    transform-style: preserve-3d;

    &-item {
        position: absolute;
        top: 0;
        left: 0;
        transition: transform 1s ease-in-out;
    }
}

@keyframes rotate {
    0% {
        transform: rotateY(0);
    }
    100% {
        transform: rotateY(360deg);
    }
}
</style>