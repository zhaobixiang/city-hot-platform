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
// import * as THREE from 'three';
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
            // zIndex: 130,
            zIndex: 10,
            zooms: [16, 20],
            // visible: false,
            // opacity: 0,
        });

        buildingLayer.setStyle({
            hideWithoutStyle: true,
            areas: [{
                rejectTexture: true,
                // color1: '072d5d', //顶面
                // color2: '062b5b', 
                path: [
                    [102.637337, 37.947473],
                    // [102.6641608, 37.947495],
                    // [102.641945, 37.945058],
                    // [102.637764, 37.944807],
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
                canvasLayer,
            ]
        });

        let camera, renderer, scene;
        var meshes = [];
        var customCoords = map.customCoords;
        // 数据使用转换工具进行转换，这个操作必须要提前执行（在获取镜头参数 函数之前执行），否则将会获得一个错误信息。
        var data = customCoords.lngLatsToCoords([
            [102.638453, 37.944972],
            [102.639769, 37.945106]
            // [116.52, 39.79],
            // [116.54, 39.79],
            // [116.56, 39.79],
        ]);

        var glLayer = new AMap.GLCustomLayer({
            zIndex: 200,
            init: (gl) => {
                camera = new THREE.PerspectiveCamera(
                    60,
                    window.innerWidth / window.innerHeight,
                    100,
                    8000
                );

                renderer = new THREE.WebGLRenderer({
                    context: gl, // 地图的 gl 上下文
                    // alpha: true,
                    // antialias: true,
                    // canvas: gl.canvas,
                });

                // 自动清空画布这里必须设置为 false，否则地图底图将无法显示
                renderer.autoClear = false;
                scene = new THREE.Scene();

                // 环境光照和平行光
                var aLight = new THREE.AmbientLight(0xffffff, 0.3);
                var dLight = new THREE.DirectionalLight(0xffffff, 1);
                dLight.position.set(1000, -100, 900);
                scene.add(dLight);
                scene.add(aLight);

                var texture = new THREE.TextureLoader().load(
                    'https://a.amap.com/jsapi_demos/static/demo-center-v2/three.jpeg'
                );
                texture.minFilter = THREE.LinearFilter;
                //  这里可以使用 three 的各种材质
                var mat = new THREE.MeshPhongMaterial({
                    color: 0xfff0f0,
                    depthTest: true,
                    transparent: true,
                    // map: texture,
                });

                var geo = new THREE.BoxBufferGeometry(100, 15, 25);
                for (let i = 0; i < data.length; i++) {
                    const d = data[i];
                    var mesh = new THREE.Mesh(geo, mat);
                    mesh.position.set(d[0], d[1], 0);
                    mesh.rotateZ(Math.PI / 22);
                    meshes.push({
                        mesh,
                        count: i,
                    });
                    scene.add(mesh);
                }

            },
            render: () => {
                // 这里必须执行！！重新设置 three 的 gl 上下文状态。
                renderer.resetState();
                // 重新设置图层的渲染中心点，将模型等物体的渲染中心点重置
                // 否则和 LOCA 可视化等多个图层能力使用的时候会出现物体位置偏移的问题
                // customCoords.setCenter([116.52, 39.79]);
                customCoords.setCenter([102.638453, 37.944972]);
                // customCoords.setCenter([102.6398, 37.9459]);
                var { near, far, fov, up, lookAt, position } =
                    customCoords.getCameraParams();

                // 2D 地图下使用的正交相机
                // var { near, far, top, bottom, left, right, position, rotation } = customCoords.getCameraParams();

                // console.log(position)

                // 这里的顺序不能颠倒，否则可能会出现绘制卡顿的效果。
                camera.near = near;
                camera.far = far;
                camera.fov = fov;
                camera.position.set(...position);
                // camera.position.set(200, -500, 500);
                camera.up.set(...up);
                camera.lookAt(...lookAt);
                camera.updateProjectionMatrix();

                // 2D 地图使用的正交相机参数赋值
                // camera.top = top;
                // camera.bottom = bottom;
                // camera.left = left;
                // camera.right = right;
                // camera.position.set(...position);
                // camera.updateProjectionMatrix();

                renderer.render(scene, camera);

                // 这里必须执行！！重新设置 three 的 gl 上下文状态。
                renderer.resetState();
            }
        });

        map.add(glLayer);

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