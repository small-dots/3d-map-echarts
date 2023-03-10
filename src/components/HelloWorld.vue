<script setup>
import { onMounted, ref } from 'vue'
import * as echarts from 'echarts'
import 'echarts-gl'
import jsonData from './map.json'
import detailTexture from '../assets/1024.png'
onMounted(() => {
  console.log(echarts.graphic)
  // const texture = new echarts.graphic.Texture({
  //   image: detailTexture,
  //   repeat: 'repeat',
  // })
  // const meterial = new echarts.graphic.Material({
  //   roughness: 0.5,
  //   normalMap: texture,
  //   normalScale: 0.1,
  //   texture: texture,
  // })
  // const meterial = new echarts.graphic.Material({
  //   shader: 'ecgl',
  //   uniforms: {
  //     texture: texture,
  //     diffuse: [1, 1, 1, 1],
  //     ambient: [0.2, 0.2, 0.2, 1],
  //     specular: [0.5, 0.5, 0.5, 1],
  //     shininess: 40,
  //   },
  // })
  const dom = document.getElementById('maper')
  const myChart = echarts.init(dom)
  echarts.registerMap('德兴市', jsonData)
  const regions = jsonData.features.map(function (feature) {
    return {
      name: feature.properties.name,
      value: Math.max(Math.sqrt(feature.properties.height), 0.1),
      height: Math.max(Math.sqrt(feature.properties.height), 0.1),
    }
  })
  console.log(regions)
  myChart.setOption({
    series: [
      // {
      //   type: 'map3D',
      //   map: '德兴市',
      //   shading: 'realistic', // 真实感渲染，配合 light.ambientCubemap 和 postEffect 使用可以让展示的画面效果和质感有质的提升
      //   // boxDepth: 180, //地图倾斜度
      //   regionHeight: 15, //地图高度
      //   // 真实感材质相关的配置项
      //   realisticMaterial: {
      //     roughness: 1, //表示材质的粗糙度，0为完全光滑，1完全粗糙，中间的值则是介于这两者之间。
      //     textureTiling: [1, 1], //材质细节纹理的平铺。默认为1，也就是拉伸填满。大于 1 的时候，数字表示纹理平铺重复的次数。
      //     detailTexture: detailTexture, //纹理贴图。
      //     // normalTexture: detailTexture,
      //     textureOffset: 0,
      //     roughness: 0, // 粗糙度
      //     metalness: 0,
      //     roughnessAdjust: 0,
      //     metalnessAdjust: 0,
      //   },
      //   // 后处理特效的相关配置项，为画面添加高光、景深、环境光遮蔽（SSAO）、调色等效果
      //   // postEffect: {
      //   //   enable: true,
      //   //   bloom: {
      //   //     enable: false, // 高光特效
      //   //   },
      //   //   //环境光遮蔽
      //   //   SSAO: {
      //   //     enable: true,
      //   //     quality: 'medium', //遮蔽的质量
      //   //     radius: 10,
      //   //     intensity: 1.2,
      //   //   },
      //   //   //景深效果 景深效果是模拟摄像机的光学成像效果，在对焦的区域相对清晰，离对焦的区域越远则会逐渐模糊。
      //   //   depthOfField: {
      //   //     enable: false,
      //   //     focalRange: 5,
      //   //     fstop: 1,
      //   //     blurRadius: 6,
      //   //   },
      //   // },
      //   // // 地面
      //   // groundPlane: {
      //   //   show: true,
      //   //   color: '#333',
      //   // },
      //   // 、、 光照相关的设置
      //   // light: {
      //   //   // 场景主光源
      //   //   main: {
      //   //     intensity: 6, //主光源的强度
      //   //     shadow: true, // 主光源是否投射阴影
      //   //     shadowQuality: 'high',
      //   //     alpha: 30, //主光源绕 x 轴，即上下旋转的角度
      //   //   },
      //   //   //全局的环境光设置
      //   //   ambient: {
      //   //     intensity: 0, //环境光的强度
      //   //   },
      //   //   //使用纹理作为环境光的光源
      //   //   ambientCubemap: {
      //   //     // texture: 'https://echarts.apache.org/examples/data-gl/asset/canyon.hdr',
      //   //     exposure: 2,
      //   //     diffuseIntensity: 1,//漫反射的强度
      //   //     specularIntensity: 1,//高光反射的强度
      //   //   },
      //   // },
      //   // 用于鼠标的旋转，缩放等视角控制
      //   viewControl: {
      //     projection: 'perspective', // 透视投影
      //     panSensitivity: 1, // 平移灵敏度
      //     panMouseButton: 'right', // 平移鼠标按键
      //     minBeta: -360,
      //     maxBeta: 360,
      //     distance: 150, //地图视角 控制初始大小
      //   },
      //   itemStyle: {
      //     areaColor: '#fff',
      //   },
      //   label: {
      //     color: '#fff',
      //   },
      //   silent: true,
      //   instancing: true,
      //   boxWidth: 200,
      //   boxHeight: 100,
      //   data: regions,
      // },
      {
        type: 'map3D',
        name: '德兴市',
        width: 1024,
        height: 1024,
        selectedMode: 'single', //地图高亮单选
        boxDepth: 150, //地图倾斜度
        regionHeight: 1, //地图高度
        map: '德兴市',
        viewControl: {
          center: [-5, -18, 0],
          projection: 'perspective', // 投影方式
          distance: 150, //地图视角 控制初始大小
          rotateSensitivity: true, //旋转
          zoomSensitivity: true, //缩放
          panSensitivity: 1, // 平移灵敏度
          panMouseButton: 'right', // 平移鼠标按键
        },
        instancing: true,
        label: {
          show: true, //是否显示市
          color: '#fff', //文字颜色
          fontSize: 18, //文字大小
          offset: [40, 30], //坐标偏移
          fontWeight: '400', //文字粗细
          backgroundColor: 'rgba(0,0,0,0)', //透明度0清空文字背景
          distance: 0, //文字距离地图距离
          formatter: function (params) {
            return params.name
          },
        },
        itemStyle: {
          color: 'rgb(255,255,255)', //地图颜色 需要用纯白色，地图的主色有纹理来控制
          borderWidth: 2.5, //分界线wdith
          opacity: 1, //地图透明度
          borderColor: 'rgba(192,245,249,1)', //分界线颜色
        },
        shading: 'color', // 着色效果
        // 着色材质、纹理
        colorMaterial: {
          // 纹理贴图
          textureOffset: 0,
          detailTexture: detailTexture,
          textureTiling: 12, // 纹理平铺
          roughness: 0, // 粗糙度
          metalness: 0,
          roughnessAdjust: 0,
          metalnessAdjust: 0,
          //  normalTexture: detailTexture, // 法线纹理
        },
        light: {
          main: {
            color: '#fff', // 光照颜色
            intensity: 1.2, // 光照强度
            // shadow: true, // 是否产生阴影
          },
        },
      },
      {
        type: 'map3D',
        name: '德兴市',
        width: 1020,
        height: 1020,
        selectedMode: 'single', //地图高亮单选
        boxDepth: 150, //地图倾斜度
        regionHeight: 1, //地图高度
        map: '德兴市',
        viewControl: {
          center: [-5, -18, 0],
          projection: 'perspective', // 投影方式
          distance: 150, //地图视角 控制初始大小
          rotateSensitivity: true, //旋转
          zoomSensitivity: true, //缩放
          panSensitivity: 1, // 平移灵敏度
          panMouseButton: 'right', // 平移鼠标按键
        },
        instancing: true,
        label: {
          show: true, //是否显示市
          color: '#fff', //文字颜色
          fontSize: 18, //文字大小
          offset: [40, 30], //坐标偏移
          fontWeight: '400', //文字粗细
          backgroundColor: 'rgba(0,0,0,0)', //透明度0清空文字背景
          distance: 0, //文字距离地图距离
          formatter: function (params) {
            return params.name
          },
        },
        itemStyle: {
          color: 'rgb(255,255,255)', //地图颜色 需要用纯白色，地图的主色有纹理来控制
          borderWidth: 2.5, //分界线wdith
          opacity: 1, //地图透明度
          borderColor: 'rgba(192,245,249,1)', //分界线颜色
        },
        shading: 'realistic', // 着色效果
        // 着色材质、纹理
        realisticMaterial: {
          // 纹理贴图
          textureOffset: 0,
          detailTexture: detailTexture,
          textureTiling: [1, 1],
          roughness: 0,
          //  normalTexture: detailTexture, // 法线纹理
        },
        light: {
          main: {
            color: '#fff', // 光照颜色
            intensity: 1.2, // 光照强度
            // shadow: true, // 是否产生阴影
          },
        },
      },
    ],
  })
})
</script>

<template>
  <div id="maper"></div>
</template>

<style scoped>
#maper {
  width: 1024px;
  height: 1024px;
}
</style>
