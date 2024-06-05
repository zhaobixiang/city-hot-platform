<template>
    <div class="home">
        <Base v-show="!showFloor" @go-floor="goFloor" />
        <Floor v-show="showFloor" @hide="showFloor = false" />
    </div>
</template>

<script setup>
import { ref } from 'vue'
import Base from './base.vue'
import Floor from './floor.vue'

const showFloor = ref(false);

const goFloor = () => {
    showFloor.value = true;
}
</script>

<style lang="scss" scoped>
.home {
    width: 100%;
    height: 100%;
}
</style>