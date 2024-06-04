<template>
    <div class="home">
        <div class="home-toolbar">
            <div class="home-toolbar-title">碧水兰庭基本信息</div>
            <div class="home-toolbar-right">
                <el-dropdown>
                    <div class="home-toolbar-select">
                        环路筛选
                        <el-icon class="home-toolbar-select-icon">
                            <arrow-down />
                        </el-icon>
                    </div>
                    
                    <template #dropdown>
                        <el-dropdown-menu>
                            <el-dropdown-item>环路</el-dropdown-item>
                        </el-dropdown-menu>
                    </template>
                </el-dropdown>

                <div class="home-toolbar-right-line"></div>

                <el-dropdown>
                    <div class="home-toolbar-select">
                        参数筛选
                        <el-icon class="home-toolbar-select-icon">
                            <arrow-down />
                        </el-icon>
                    </div>
                    
                    <template #dropdown>
                        <el-dropdown-menu>
                            <el-dropdown-item>参数</el-dropdown-item>
                        </el-dropdown-menu>
                    </template>
                </el-dropdown>

                <div class="home-toolbar-right-line"></div>

                <el-dropdown>
                    <div class="home-toolbar-select">
                        倾斜模式
                        <el-icon class="home-toolbar-select-icon">
                            <arrow-down />
                        </el-icon>
                    </div>
                    
                    <template #dropdown>
                        <el-dropdown-menu>
                            <el-dropdown-item>倾斜模式</el-dropdown-item>
                            <el-dropdown-item>标准模式</el-dropdown-item>
                        </el-dropdown-menu>
                    </template>
                </el-dropdown>

                <div class="home-toolbar-right-line"></div>

                <div class="home-toolbar-right-eye" @click="showDialog = !showDialog">
                    <img :src="eyeImg" />
                    <div class="close" v-show="!showDialog"></div>
                </div>
            </div>
        </div>
        <div id="map"></div>
        
        <!-- <div class="home-mask"></div> -->
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { ArrowDown } from '@element-plus/icons-vue';
import AMapLoader from '@amap/amap-jsapi-loader';
import eyeImg from '@/assets/eye.png';

const showDialog = ref(true);



onMounted(() => {
    window._AMapSecurityConfig = {
        securityJsCode: 'b0f14636e082801bf255f5ad23eadeb9',
    };

    AMapLoader.load({
        key: '2291cfc955f46af970fd00847bf1529e',
        version: '2.0',
        plugins: []
    }).then((AMap) => {
        const buildingLayer = new AMap.Buildings({
            zIndex: 130,
            zooms: [16, 20],
        });

        buildingLayer.setStyle({
            hideWithoutStyle: false,
            areas: [{
                rejectTexture: true,
                color1: '69ada0', //顶面
                color2: '4c4c4c', 
                path: [
                    [102.637337, 37.947473],
                    [102.6641608, 37.947495],
                    [102.641945, 37.945058],
                    [102.637764, 37.944807],
                ]
            }]
        });

        var canvas = document.createElement('canvas');
 
        // 创建context对象
        var ctx = canvas.getContext('2d');
        
        // 设置画布颜色为红色
        // ctx.fillStyle = '#072e5e';
        ctx.fillStyle = '#002859';
        
        // 填充整个画布
        ctx.fillRect(0, 0, 1920, 1680);

        var canvasLayer = new AMap.CanvasLayer({
            canvas: canvas,
            bounds: new AMap.Bounds(
                // [102.637337, 37.947473],
                // [102.641945, 37.945058],
                [80, 50],
                [120, 20]
            ),
            opacity: 0.8,
            zooms: [16, 20],
        });

        const map = new AMap.Map('map', {
            viewMode: '3D',
            center: [102.6398, 37.9459],
            // zoom: 18.8,
            zoom: 18.8,
            pitch: 45,
            // rotation: 20,
            mapStyle: "amap://styles/blue",
            features: ['bg', 'road', 'building'],
            layers: [
                AMap.createDefaultLayer(),
                buildingLayer,
                // customLayer,
                // imageLayer
                canvasLayer
            ]
        });
    }).catch(e => {
        console.log(e)
    });
});
</script>

<style lang="scss" scoped>
.home {
    width: 100%;
    height: 100%;
    position: relative;
    overflow: hidden;

    &-toolbar {
        position: fixed;
        top: 0;
        left: 0;
        width: calc(100% - 40px);
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 0 20px;
        height: 60px;
        background-color: #04225f;
        box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
        color: #fff;
        font-size: 16px;
        z-index: 9;

        &-title {
            margin-left: 20px;
            font-size: 28px;
            line-height: 60px;
            margin-right: 30px;
            color: #fff;
        }

        &-right {
            display: flex;
            align-items: center;

            &-line {
                width: 1px;
                height: 20px;
                background: #333;
                margin: 0 20px
            }

            &-eye {
                position: relative;
                cursor: pointer;

                img {
                    width: 25px;
                    height: 17px;
                }

                .close {
                    position: absolute;
                    top: -6px;
                    left: 12px;
                    width: 2px;
                    height: 30px;
                    background-color: #fff;
                    transform: rotate(30deg);
                }
            }
        }

        &-select {
            padding: 0 12px;
            border: 1px solid #ccc;
            background-color: #dddddd52;
            border-radius: 12px;
            height: 24px;
            line-height: 25px;
            color: #fff;
            display: flex;
            align-items: center;

            &.active {
                border-color: #dff1ff;
                background-color: #0f98e0;
            }

            &-icon {
                margin-left: 5px;
            }
        }
    }

    #map {
        width: 100%;
        height: 100%;
    }

    &-mask {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(1, 19, 78, 0.6);
        pointer-events: none;
    }
}
</style>