<template>
    <div class="home">
        <div class="home-toolbar">
            <div class="home-toolbar-left">
                <div class="home-toolbar-title">碧水兰庭基本信息</div>
                <div class="home-toolbar-info">
                    <div class="home-toolbar-info-label">热网面积</div>
                    <div class="home-toolbar-info-value">{{ floorData.d1 }}</div>
                    <div class="home-toolbar-info-label">供热面积</div>
                    <div class="home-toolbar-info-value">{{ floorData.d2 }}</div>
                </div>

                <div class="home-toolbar-info user">
                    <div class="home-toolbar-info-label">正常供热</div>
                    <div class="home-toolbar-info-value">{{ floorData.d3 }}</div>
                    <div class="home-toolbar-info-label small">停供</div>
                    <div class="home-toolbar-info-value">{{ floorData.d4 }}</div>
                    <div class="home-toolbar-info-label small">欠费</div>
                    <div class="home-toolbar-info-value">{{ floorData.d5 }}</div>
                </div>

                <div class="home-toolbar-info temp">
                    <div class="home-toolbar-info-label">18℃以下</div>
                    <div class="home-toolbar-info-value">{{ floorData.d6 }}</div>
                    <div class="home-toolbar-info-label">18℃ -24℃</div>
                    <div class="home-toolbar-info-value">{{ floorData.d7 }}</div>
                    <div class="home-toolbar-info-label">24℃以上</div>
                    <div class="home-toolbar-info-value">{{ floorData.d8 }}</div>
                </div>
            </div>
            
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
                            <el-dropdown-item>阀位</el-dropdown-item>
                            <el-dropdown-item>回温</el-dropdown-item>
                            <el-dropdown-item>室温</el-dropdown-item>
                        </el-dropdown-menu>
                    </template>
                </el-dropdown>

                <div class="home-toolbar-right-line"></div>

                <el-dropdown @command="changeMode">
                    <div class="home-toolbar-select">
                        {{ mode === '1' ? '倾斜模式' : '标准模式' }}
                        <el-icon class="home-toolbar-select-icon">
                            <arrow-down />
                        </el-icon>
                    </div>
                    
                    <template #dropdown>
                        <el-dropdown-menu>
                            <el-dropdown-item command="1">倾斜模式</el-dropdown-item>
                            <el-dropdown-item command="2">标准模式</el-dropdown-item>
                        </el-dropdown-menu>
                    </template>
                </el-dropdown>

                <div class="home-toolbar-right-line"></div>

                <div class="home-toolbar-right-eye" @click="toggle">
                    <img :src="eyeImg" />
                    <div class="close" v-show="!showDialog"></div>
                </div>
            </div>
        </div>
        <div id="map"></div>
        
        <marker-info ref="markerPopover" />
        <!-- <div class="home-mask"></div> -->
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { ArrowDown } from '@element-plus/icons-vue';
import AMapLoader from '@amap/amap-jsapi-loader';
import MarkerInfo from '@/components/marker-info.vue';
import eyeImg from '@/assets/eye.png';
import marker1 from '@/assets/marker1.png';
import marker2 from '@/assets/marker2.png';
import marker3 from '@/assets/marker3.png';

import geoJson from './geo.json';
import { list_2d, list_3d } from './data.js';

const emit = defineEmits(['go-floor']);

let $AMap, $map, $loca;
const mode = ref('1');
const showDialog = ref(true);
const markerPopover = ref();
const allMarkers = [];
const allMarker1s = [];
const allTexts = [];
const markerIcons = ['', marker1, marker2, marker3];

const floorData = ref({
    d1: '24588m²',
    d2: '10488m²',
    d3: '165户',
    d4: '22户',
    d5: '7户',
    d6: '7户',
    d7: '127户',
    d8: '22户',
    total: 14, // 楼数量
    layer: 10, // 层数
    unit: 3, // 单元数
    door: 4, // 户数
});

onMounted(() => {
    // emit('go-floor');

    window._AMapSecurityConfig = {
        securityJsCode: 'b0f14636e082801bf255f5ad23eadeb9',
    };

    AMapLoader.load({
        key: '2291cfc955f46af970fd00847bf1529e',
        version: '2.0',
        plugins: [],
        Loca: {               
            "version": '2.0.0'
        },
    }).then((AMap) => {
        $AMap = AMap;

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

        const width = window.innerWidth;
        const zoom = width >= 1920 ? 18.5 : width >= 1680 ? 18.3 : 18;

        console.log(zoom)

        const map = new AMap.Map('map', {
            viewMode: '3D',
            center: [102.6401, 37.94628],
            // zoom: 18.8,
            zoom,
            pitch: 45,
            // pitch: 10,
            dragEnable: false,
            zoomEnable: false,
            scrollWheel: false,
            doubleClickZoom: false,
            showBuildingBlock: false,

            mapStyle: "amap://styles/blue",
            features: ['bg', 'road', 'building'],
            layers: [
                AMap.createDefaultLayer(),
                canvasLayer
            ]
        });

        const loca = new Loca.Container({
            map: map
        });

        loca.ambLight = {
            intensity: 0.3,
            color: '#fff',
        };

        // loca.dirLight = {
        //     intensity: 0.6,
        //     color: '#fff',
        //     target: [0, 0, 0],
        //     position: [0, -1, 1],
        // };

        loca.pointLight = {
            color: 'rgb(100, 100, 100)',
            position: [102.6401, 37.94628, 20000],
            intensity: 0.5,
            distance: 50000,
        };

        const geo = new Loca.GeoJSONSource({
            // data: geoData
            data: geoJson
        });

        var pl = new Loca.PolygonLayer({
            zIndex: 200,
            cullface: 'none',
            shininess: 10,
            // labelsLayerOptions: {
            //     zIndex: 199,
            //     collision: true
            // }
        });

        pl.setSource(geo);

        pl.setStyle({
            topColor: (_, feature) => {
                return feature.properties.color;
            },
            // sideTopColor: '#4c4c4c',
            // sideBottomColor: '#4c4c4c',
            sideTopColor: '#666',
            sideBottomColor: '#666',
            altitude: 0,
            height: (_, feature) => {
                return feature.properties.height;
            },
            // label: {
            //     // zIndex: 1,
            //     text: {
            //         content: (_, feature) => feature.properties.name || '1栋1单元 26.7℃',
            //         // offset: [0, 10],
            //         direction: 'center',
            //         style: {
            //             fontSize: 18,
            //             fillColor: '#4c4c4c',
            //         }
            //     }
            // },
        });
        
        loca.add(pl);

        map.on('dblclick', e => {
            // console.log(e)

            const feat = pl.queryFeature(e.pixel.toArray());

            // console.log(feat)

            if (feat) {
                emit('go-floor');
            }
        });

        // map.on('dblclick', e => {
        //     // console.log(e)

        //     const pos = e.lnglat;
        //     // 点击在楼范围内
        //     const floor = areas.find(v => AMap.GeometryUtil.isPointInRing(pos, v.path));

        //     if (floor) {
        //         emit('go-floor');
        //     }
        // });

        createMarkerText(AMap, map);

        $map = map;
        $loca = loca;
    }).catch(e => {
        console.log(e)
    });
});

const createMarkerText = (AMap, map) => {
    list_3d.forEach(item => {
        item.text.forEach(tp => {
            var text = new AMap.Text({
                text: '1栋1单元 26.7℃',
                angle: item.angle ?? -6,
                style:{
                    'background-color': 'transparent',
                    'border-width': 0,
                    'font-size': item.fontSize ?? '18px',
                    'color': '#4c4c4c'
                },
                clickable: false,
                zIndex: 15,
                position: tp
            });

            allTexts.push(text);
            text.setMap(map);
        });

        item.marker.forEach((dot, index) => {
            var icon = markerIcons[item.icon?.[index] ?? 1];
            var marker = new AMap.Marker({
                // position: new AMap.LngLat(102.637925, 37.945126),
                position: new AMap.LngLat(...dot),
                // anchor: 'bottom-center',
                content: `<div class="floor-marker">
                        <div class="floor-marker-box">
                            <div class="floor-marker-box-header">单元号: 2103</div>
                            <div class="floor-marker-box-body">
                                <div>阀位: 52%</div>
                                <div>回温: 37.9℃</div>
                                <div>流量: 60.4t/h</div>
                            </div>
                        </div><div class="floor-marker-box1" style="display: none;">
                            <div class="floor-marker-box1-header">单元号: 2103</div>
                            <div class="floor-marker-box1-body">
                                <div>阀位: 52%</div>
                                <div>回温: 37.9℃</div>
                                <div>流量: 60.4t/h</div>
                            </div>
                        </div>
                        <div class="floor-marker-dot"></div>
                        <div class="floor-marker-line"></div>
                        <img class="floor-marker-img" src=${icon} />
                    </div>`,
                offset: new AMap.Pixel(0, -40),
                zIndex: 16,
            });

            allMarkers.push(marker);
            map.add(marker);
        });
    });

    const markers = document.querySelectorAll('.floor-marker-img');
    const boxs = document.querySelectorAll('.floor-marker-box');
    const box1s = document.querySelectorAll('.floor-marker-box1');

    [...markers].forEach(node => {
        node.addEventListener('dblclick', (e) => {
            console.log(e)
            markerPopover.value.show({ x: e.x, y: e.y });
        });
    });

    [...boxs].forEach(node => {
        node.addEventListener('mousemove', (e) => {
            const self = e.currentTarget;
            const next = self.nextSibling;

            if (self) {
                self.style.display = 'none';
            }
            if (next) {
                next.style.display = '';
            }
        });
    });

    [...box1s].forEach(node => {
        node.addEventListener('mouseleave', (e) => {
            const self = e.currentTarget;
            const prev = self.previousElementSibling;

            if (self) {
                self.style.display = 'none';
            }
            if (prev) {
                prev.style.display = '';
            }
        });
    });

}

const createMarkerText1 = (AMap, map) => {
    list_2d.forEach(item => {
        item.text.forEach(tp => {
            var text = new AMap.Text({
                text: '1栋1单元 26.7℃',
                angle: item.angle ?? -6,
                style:{
                    'background-color': 'transparent',
                    'border-width': 0,
                    'font-size': item.fontSize ?? '18px',
                    'color': '#4c4c4c'
                },
                clickable: false,
                zIndex: 15,
                position: tp
            });

            allTexts.push(text);
            text.setMap(map);
        });

        item.marker.forEach((dot, index) => {
            var icon = markerIcons[item.icon?.[index] ?? 1];
            var marker = new AMap.Marker({
                // position: new AMap.LngLat(102.637925, 37.945126),
                position: new AMap.LngLat(...dot),
                // anchor: 'bottom-center',
                content: `<div class="floor-marker">
                        <div class="floor-marker-box">
                            <div class="floor-marker-box-header">单元号: 2103</div>
                            <div class="floor-marker-box-body">
                                <div>阀位: 52%</div>
                                <div>回温: 37.9℃</div>
                                <div>流量: 60.4t/h</div>
                            </div>
                        </div><div class="floor-marker-box1" style="display: none;">
                            <div class="floor-marker-box1-header">单元号: 2103</div>
                            <div class="floor-marker-box1-body">
                                <div>阀位: 52%</div>
                                <div>回温: 37.9℃</div>
                                <div>流量: 60.4t/h</div>
                            </div>
                        </div>
                        <div class="floor-marker-dot"></div>
                        <div class="floor-marker-line"></div>
                        <img class="floor-marker-img" src=${icon} />
                    </div>`,
                offset: new AMap.Pixel(0, -40),
                zIndex: 16,
            });

            allMarker1s.push(marker);
            map.add(marker);
        });
    });

    const markers = document.querySelectorAll('.floor-marker-img');
    const boxs = document.querySelectorAll('.floor-marker-box');
    const box1s = document.querySelectorAll('.floor-marker-box1');

    [...markers].forEach(node => {
        node.addEventListener('dblclick', (e) => {
            console.log(e)
            markerPopover.value.show({ x: e.x, y: e.y });
        });
    });

    [...boxs].forEach(node => {
        node.addEventListener('mousemove', (e) => {
            const self = e.currentTarget;
            const next = self.nextSibling;

            if (self) {
                self.style.display = 'none';
            }
            if (next) {
                next.style.display = '';
            }
        });
    });

    [...box1s].forEach(node => {
        node.addEventListener('mouseleave', (e) => {
            const self = e.currentTarget;
            const prev = self.previousElementSibling;

            if (self) {
                self.style.display = 'none';
            }
            if (prev) {
                prev.style.display = '';
            }
        });
    });

}

const toggle = () => {
    const current = !showDialog.value;

    showDialog.value = current;

    if (mode.value === '1') {
        allMarkers.forEach(v => {
            v.setMap(current ? $map : null);
        });
    } else {
        allMarker1s.forEach(v => {
            v.setMap(current ? $map : null);
        });
    }
}

const changeMode = (v) => {
    // console.log(v);

    const lastValue = mode.value;

    mode.value = v;
    showDialog.value = true;

    // 倾斜模式
    if (v === '1') {
        if (lastValue === v) {
            return;
        }

        $map.setPitch(45);

        // 清除
        [...allMarkers, ...allMarker1s].forEach(v => {
            v.setMap(null);
        });
        allTexts.forEach(v => {
            v.setMap(null);
        });

        createMarkerText($AMap, $map);
    }
    // 标准模式
    if (v === '2') {
        if (lastValue === v) {
            return;
        }

        $map.setPitch(10);

        // 清除
        [...allMarkers, ...allMarker1s].forEach(v => {
            v.setMap(null);
        });
        allTexts.forEach(v => {
            v.setMap(null);
        });

        createMarkerText1($AMap, $map);
    }
}
</script>

<style lang="scss">
.floor-marker {
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    &-box {
        position: absolute;
        top: -102px;
        left: -35px;
        min-width: 100px;
        min-height: 80px;
        border: 1px solid #999;
        border-radius: 10px;
        background-color: rgba(0, 0, 0, 0.5);
        color: #fff;
        font-size: 12px;

        &-header {
            padding: 0 10px;
            background-color: #3b414b;
            line-height: 26px;
            border-top-left-radius: 10px;
            border-top-right-radius: 10px;
        }

        &-body {
            padding: 6px 10px;
            line-height: 20px;
        }
    }

    &-box1 {
        position: absolute;
        top: -137px;
        left: -68px;
        min-width: 160px;
        min-height: 120px;
        border: 1px solid #24a0b8;
        background-color: rgba(0, 0, 0, 0.5);
        color: #fff;
        font-size: 14px;

        &-header {
            padding: 0 16px;
            height: 36px;
            line-height: 36px;
            border-bottom: 1px solid #24a0b8;
        }

        &-body {
            padding: 6px 16px;
            line-height: 28px;
        }
    }

    &-dot {
        width: 10px;
        height: 10px;
        border-radius: 50%;
        background-color: #24e0bb;
        margin-top: -5px;
        z-index: 2;
    }
    
    &-line {
        width: 1px;
        height: 30px;
        background-color: #3f7a6f;
    }
}
</style>
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

        &-left {
            display: flex;
            align-items: center;
        }

        &-title {
            margin-left: 0.5rem;
            font-size: 1.5rem;
            margin-right: 1.2rem;
            color: #fff;
        }

        &-info {
            display: flex;
            align-items: center;
            font-size: 12px;
            padding-left: 50px;
            height: 35px;
            background: url('@/assets/area.png') no-repeat;

            &.user {
                background: url('@/assets/user.png') no-repeat;
            }
            &.temp {
                background: url('@/assets/temp.png') no-repeat;
            }

            &-label {
                width: 30px;
                margin-right: 5px;

                &.small {
                    width: 12px;
                    margin-right: 10px;
                }

                &.large {
                    width: 35px;
                }
            }

            &-value {
                font-size: 1rem;
                margin-right: 10px;
                font-weight: 600;
            }
        }

        &-right {
            display: flex;
            align-items: center;

            &-line {
                width: 1px;
                height: 20px;
                background: #333;
                margin: 0 1rem;
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
            // line-height: 25px;
            color: #fff;
            display: flex;
            align-items: center;
            font-size: 0.8rem;

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