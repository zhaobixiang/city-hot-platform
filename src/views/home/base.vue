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

                <div class="home-toolbar-right-eye" @click="toggle">
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
import marker1 from '@/assets/marker1.png';

const emit = defineEmits(['go-floor']);

let $map;
const showDialog = ref(true);
const allMarkers = [];

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

        // [
        //     [102.637337, 37.947473],
        //     [102.6641608, 37.947495],
        //     [102.641945, 37.945058],
        //     [102.637764, 37.944807],
        // ]

        const areas = [{
            rejectTexture: true,
            color1: 'df9b87', //顶面
            color2: '4c4c4c', 
            path: [
                [102.637925, 37.945016],
                [102.639045, 37.945123],
                [102.639072, 37.944965],
                [102.637977, 37.944821],
            ],
            textPoint: [[102.637925, 37.945126]],
            dotList: [
                [102.637925, 37.945166],
                [102.638225, 37.945186],
                [102.638625, 37.945246],
            ],
        }, {
            rejectTexture: true,
            color1: '84aed9', //顶面
            color2: '4c4c4c', 
            path: [
                [102.637733,37.945333],
                [102.638514,37.945429],
                [102.638527,37.945274],
                [102.637764,37.945181],
            ],
            textPoint: [[102.637563,37.945622]],
            dotList: [
                [102.637403,37.945652],
                [102.637903,37.945722],
            ],
        }, {
            rejectTexture: true,
            color1: '69ada0', //顶面
            color2: '4c4c4c', 
            path: [
                [102.637699,37.945643],
                [102.638469,37.945769],
                [102.638493,37.945586],
                [102.637728,37.945475],
            ],
            textPoint: [[102.63679,37.94688]],
            dotList: [
                [102.63656,37.94688],
                [102.63703,37.94696],
            ],
        }, {
            rejectTexture: true,
            color1: 'df9b87', //顶面
            color2: '4c4c4c', 
            path: [
                [102.637531,37.946343],
                [102.638631,37.946506],
                [102.638762,37.945933],
                [102.637652,37.945785],
            ],
            textPoint: [[102.63688, 37.9474]],
            dotList: [
                [102.6376, 37.94756],
            ],
        }, {
            rejectTexture: true,
            color1: '69ada0', //顶面
            color2: '4c4c4c', 
            path: [
                [102.638545,37.94546],
                [102.639164,37.945539],
                [102.639184,37.945314],
                [102.638603,37.945221],
            ],
            textPoint: [[102.63849,37.9456]],
            dotList: [],
        }, {
            rejectTexture: true,
            color1: '84aed9', //顶面
            color2: '4c4c4c', 
            path: [
                [102.638488,37.945756],
                [102.639195,37.945869],
                [102.639221,37.9457],
                [102.638508,37.945571],
            ],
            textPoint: [[102.63849,37.94598]],
            dotList: [
                [102.63849,37.94602]
            ],
        }, {
            rejectTexture: true,
            color1: '69ada0', //顶面
            color2: '4c4c4c', 
            path: [
                [102.638929,37.94622],
                [102.639473,37.946297],
                [102.639551,37.945992],
                [102.638995,37.945924],
            ],
            textPoint: [[102.63897,37.94628]],
            dotList: [
                [102.638929,37.94628]
            ],
            fontSize: '14px'
        }, {
            rejectTexture: true,
            color1: 'df9b87', //顶面
            color2: '4c4c4c', 
            path: [
                [102.638974,37.946948],
                [102.639487,37.946975],
                [102.639515,37.946795],
                [102.639023,37.946729],
            ],
            textPoint: [[102.63904,37.94696]],
            dotList: [
                [102.63904,37.94706]
            ],
            fontSize: '12px'
        }, {
            rejectTexture: true,
            color1: '69ada0', //顶面
            color2: '4c4c4c', 
            path: [
                [102.638539,37.947341],
                [102.639315,37.947346],
                [102.639272,37.947149],
                [102.638524,37.947152],
            ],
            textPoint: [[102.63855,37.9474]],
            dotList: [
                [102.63855,37.94746]
            ],
            angle: 0,
            fontSize: '16px'
        }, {
            rejectTexture: true,
            color1: '69ada0', //顶面
            color2: '4c4c4c', 
            path: [
                [102.639531,37.947372],
                [102.640456,37.947383],
                [102.640437,37.947089],
                [102.639497,37.947104],
            ],
            textPoint: [[102.63964,37.9474]],
            dotList: [
                [102.63964,37.94745]
            ],
            angle: 0,
        }, {
            rejectTexture: true,
            color1: '69ada0', //顶面
            color2: '4c4c4c', 
            path: [
                [102.640475,37.947395],
                [102.641018,37.947356],
                [102.641003,37.947116],
                [102.6405,37.947124],
            ],
            textPoint: [
                [102.64055,37.94743]
            ],
            dotList: [
                [102.64055,37.94743]
            ],
            angle: 0,
            fontSize: '12px'
        }, {
            rejectTexture: true,
            color1: '84aed9', //顶面
            color2: '4c4c4c', 
            path: [
                [102.641061,37.947333],
                [102.641608,37.947333],
                [102.641618,37.947108],
                [102.641022,37.947116],
            ],
            textPoint: [[102.64115,37.94738]],
            dotList: [],
            angle: 1,
            fontSize: '12px'
        }, {
            rejectTexture: true,
            color1: '69ada0', //顶面
            color2: '4c4c4c', 
            path: [
                [102.641647,37.947353],
                [102.642547,37.94733],
                [102.642552,37.947116],
                [102.641642,37.947108],
            ],
            textPoint: [[102.64213,37.94738]],
            dotList: [
                [102.64213,37.94742]
            ],
            angle: 1,
            fontSize: '12px'
        }, {
            rejectTexture: true,
            color1: 'df9b87', //顶面
            color2: '4c4c4c', 
            path: [
                [102.639604,37.94699],
                [102.640098,37.947047],
                [102.640122,37.946826],
                [102.639628,37.946761],
            ],
            textPoint: [[102.63967,37.94702]],
            dotList: [
                [102.63967,37.9471]
            ],
            fontSize: '12px'
        }, {
            rejectTexture: true,
            color1: '69ada0', //顶面
            color2: '4c4c4c', 
            path: [
                [102.640107,37.947017],
                [102.640446,37.94704],
                [102.640475,37.946872],
                [102.640132,37.946814],
            ],
            textPoint: [],
            dotList: [],
        }, {
            rejectTexture: true,
            color1: '84aed9', //顶面
            color2: '4c4c4c', 
            path: [
                [102.639938,37.94665],
                [102.641356,37.946795],
                [102.641381,37.946593],
                [102.639972,37.946436],
            ],
            textPoint: [[102.64032,37.94691]],
            dotList: [
                [102.63998,37.94691],
                [102.64042,37.94699],
                [102.64088,37.94705]
            ],
        }, {
            rejectTexture: true,
            color1: '69ada0', //顶面
            color2: '4c4c4c', 
            path: [
                [102.641371,37.94678],
                [102.641831,37.946822],
                [102.641865,37.946635],
                [102.641376,37.946589],
            ],
            textPoint: [[102.6418,37.94782]],
            dotList: [
                [102.6418,37.9479]
            ],
            fontSize: '14px'
        }, {
            rejectTexture: true,
            color1: '69ada0', //顶面
            color2: '4c4c4c', 
            path: [
                [102.641826,37.946833],
                [102.642668,37.946978],
                [102.642697,37.946768],
                [102.64186,37.946616],
            ],
            textPoint: [[102.64206,37.94708]],
            dotList: [],
            fontSize: '16px',
            angle: -8
        }, {
            rejectTexture: true,
            color1: '69ada0', //顶面
            color2: '4c4c4c', 
            path: [
                [102.639846,37.946245],
                [102.64125,37.946425],
                [102.64126,37.946203],
                [102.63989,37.946062],
            ],
            textPoint: [[102.64018,37.94651]],
            dotList: [],
        }, {
            rejectTexture: true,
            color1: 'df9b87', //顶面
            color2: '4c4c4c', 
            path: [
                [102.64154,37.946444],
                [102.642712,37.9466],
                [102.642746,37.946425],
                [102.641584,37.946276],
            ],
            textPoint: [[102.64185,37.94671]],
            dotList: [
                [102.64185,37.94676]
            ],
        }, {
            rejectTexture: true,
            color1: '84aed9', //顶面
            color2: '4c4c4c', 
            path: [
                [102.639328,37.945936],
                [102.640466,37.946005],
                [102.640461,37.94581],
                [102.639386,37.945707],
            ],
            textPoint: [[102.639368,37.946036]],
            dotList: [
                [102.640138,37.946128]
            ],
        }, {
            rejectTexture: true,
            color1: '84aed9', //顶面
            color2: '4c4c4c', 
            path: [
                [102.64064,37.946005],
                [102.641778,37.946119],
                [102.641792,37.945955],
                [102.640679,37.945829],
            ],
            textPoint: [[102.64094,37.946225]],
            dotList: [
                [102.64074,37.946225],
                [102.64134,37.946325],
            ],
        }, {
            rejectTexture: true,
            color1: '69ada0', //顶面
            color2: '4c4c4c', 
            path: [
                [102.639231,37.945535],
                [102.640233,37.945642],
                [102.640267,37.94547],
                [102.63927,37.945363],
            ],
            textPoint: [[102.639331,37.945935]],
            dotList: [
                [102.63946,37.94598]
            ],
        }, {
            rejectTexture: true,
            color1: '69ada0', //顶面
            color2: '4c4c4c', 
            path: [
                [102.641071,37.945726],
                [102.642058,37.945856],
                [102.642083,37.945677],
                [102.64109,37.945558],
            ],
            textPoint: [[102.641371,37.945922]],
            dotList: [
                [102.641311,37.945962],
                [102.641881,37.946042]
            ],
        }, {
            rejectTexture: true,
            color1: '69ada0', //顶面
            color2: '4c4c4c', 
            path: [
                [102.639196,37.945282],
                [102.641304,37.945493],
                [102.64139,37.945184],
                [102.639228,37.944923],
            ],
            textPoint: [
                [102.639498,37.945302],
                [102.640496,37.945502],
            ],
            dotList: [
                [102.639298,37.945302],
                [102.640196,37.945502],
                [102.640696,37.945562],
            ],
        }];

        buildingLayer.setStyle({
            hideWithoutStyle: true,
            areas
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
            center: [102.6401, 37.94628],
            // zoom: 18.8,
            zoom: 18.5,
            pitch: 45,
            // rotation: 20,
            dragEnable: false,
            zoomEnable: false,
            scrollWheel: false,
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

        map.on('dblclick', e => {
            // console.log(e)

            const pos = e.lnglat;
            // 点击在楼范围内
            const floor = areas.find(v => AMap.GeometryUtil.isPointInRing(pos, v.path));

            if (floor) {
                emit('go-floor');
            }
        });

        // var text = new AMap.Text({
        //     text: '1栋1单元 26.7℃',
        //     // anchor: 'center', // 设置文本标记锚点
        //     // draggable:true,
        //     // cursor:'pointer',
        //     angle: -6,
        //     style:{
        //         // 'padding': '.75rem 1.25rem',
        //         // 'margin-bottom': '1rem',
        //         // 'border-radius': '.25rem',
        //         'background-color': 'transparent',
        //         // 'width': '15rem',
        //         'border-width': 0,
        //         // 'box-shadow': '0 2px 6px 0 rgba(114, 124, 245, .5)',
        //         // 'text-align': 'center',
        //         'font-size': '20px',
        //         'color': '#4c4c4c'
        //     },
        //     position: [102.637925, 37.945126]
        //     // position: [102.637925, 37.945016]
        // });

        // var marker = new AMap.Marker({
        //     position: new AMap.LngLat(102.637925, 37.945166),
        //     content: `<div class="floor-marker">
        //         <div class="floor-marker-box">
        //             <div class="floor-marker-box-item">单元号: 2103</div>
        //             <div class="floor-marker-box-item">阀位: 52%</div>
        //             <div class="floor-marker-box-item">回温: 37.9℃</div>
        //             <div class="floor-marker-box-item">流量: 60.4t/h</div>
        //         </div>
        //         <div class="floor-marker-dot"></div>
        //         <div class="floor-marker-line"></div>
        //         <img class="floor-marker-img" src=${marker1} />
        //     </div>`,
        //     offset: new AMap.Pixel(0, -140)
        // });

        // map.add(marker)

        // text.setMap(map);

        createMarkerText(AMap, map, areas);

        $map = map;
    }).catch(e => {
        console.log(e)
    });
});

const createMarkerText = (AMap, map, areas) => {
    areas.forEach(item => {
        item.textPoint.forEach(tp => {
            var text = new AMap.Text({
                text: '1栋1单元 26.7℃',
                // anchor: 'center', // 设置文本标记锚点
                // draggable:true,
                // cursor:'pointer',
                angle: item.angle ?? -6,
                style:{
                    // 'padding': '.75rem 1.25rem',
                    // 'margin-bottom': '1rem',
                    // 'border-radius': '.25rem',
                    'background-color': 'transparent',
                    // 'width': '15rem',
                    'border-width': 0,
                    // 'box-shadow': '0 2px 6px 0 rgba(114, 124, 245, .5)',
                    // 'text-align': 'center',
                    'font-size': item.fontSize ?? '18px',
                    'color': '#4c4c4c'
                },
                zIndex: 15,
                position: tp,
                // position: [102.637925, 37.945126]
                // position: [102.637925, 37.945016]
            });

            text.setMap(map);
        });

        item.dotList.forEach(dot => {
            var marker = new AMap.Marker({
                // position: new AMap.LngLat(102.637925, 37.945126),
                position: new AMap.LngLat(...dot),
                content: `<div class="floor-marker">
                    <div class="floor-marker-box">
                        <div class="floor-marker-box-item">单元号: 2103</div>
                        <div class="floor-marker-box-item">阀位: 52%</div>
                        <div class="floor-marker-box-item">回温: 37.9℃</div>
                        <div class="floor-marker-box-item">流量: 60.4t/h</div>
                    </div>
                    <div class="floor-marker-dot"></div>
                    <div class="floor-marker-line"></div>
                    <img class="floor-marker-img" src=${marker1} />
                </div>`,
                offset: new AMap.Pixel(0, -140),
                zIndex: 16,
            });

            allMarkers.push(marker);
            map.add(marker);
        });
    });
}

const toggle = () => {
    const current = !showDialog.value;

    showDialog.value = current;

    allMarkers.forEach(v => {
        v.setMap(current ? $map : null);
    });
}
</script>

<style lang="scss">
.floor-marker {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    &-box {
        min-width: 80px;
        padding: 6px 10px;
        height: 80px;
        border: 1px solid #999;
        border-radius: 10px;
        background-color: rgba(0, 0, 0, 0.5);
        color: #fff;
        font-size: 12px;

        &-item {
            line-height: 20px;
        }
    }

    &-dot {
        width: 10px;
        height: 10px;
        border-radius: 50%;
        background-color: #24e0bb;
        margin-top: -5px;
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