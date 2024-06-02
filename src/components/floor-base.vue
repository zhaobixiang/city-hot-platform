<template>
    <div class="floor-base">
        <floor class="floor-base-item" v-for="v in count" :key="v" :title="`${v}号楼`" :style="styleList[v - 1]" @click="onClick(v)" />
    </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue';
import Floor from './floor.vue';

const props = defineProps({
    count: {
        type: Number,
        default: 10
    }
});

const emit = defineEmits(['change']);

const current = ref(1);
const degList = ref([]);

const styleList = computed(() => {
    return degList.value.map(v => {
        return {
            transform: `rotateY(${v}deg) translateZ(700px) rotateY(${-v}deg)`
        }
    });
});

const setDegList = (v) => {
    const num = v ?? props.count;
    const deg = 360 / num;

    let res = [];

    for (let i = 0; i < num; i++) {
        res[i] = Math.round(i * deg);
    }

    // console.log(res)

    degList.value = res;
}

const onClick = (v) => {
    console.log(v);
    const num = props.count;
    const deg = 360 / num;
    const size = v >= current.value ? v - current.value : (num + v - current.value);
    const list = degList.value;
    const len = list.length;
    let res = [];

    for (let i = 0; i < len; i++) {
        if (size > num / 2) {
            res[i] = list[i] + Math.round(deg * (num - size));
        } else {
            res[i] = list[i] - Math.round(deg * size);
        }
    }

    current.value = v;
    degList.value = res;

    emit('change', v);
}

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