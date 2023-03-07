<script setup>
import { onMounted, ref } from 'vue'
import * as echarts from 'echarts'
import 'echarts-gl'
import jsonData from './map.json'
import detailTexture from '../assets/woods.jpg'
onMounted(() => {
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
  myChart.setOption({
    series: [
      {
        type: 'map3D',
        map: '德兴市',
        shading: 'realistic', // 真实感渲染，配合 light.ambientCubemap 和 postEffect 使用可以让展示的画面效果和质感有质的提升
        // 真实感材质相关的配置项
        realisticMaterial: {
          roughness: 0.6, //表示材质的粗糙度，0为完全光滑，1完全粗糙，中间的值则是介于这两者之间。
          textureTiling: 20, //材质细节纹理的平铺。默认为1，也就是拉伸填满。大于 1 的时候，数字表示纹理平铺重复的次数。
          detailTexture: detailTexture, //纹理贴图。
        },
        // 后处理特效的相关配置项，为画面添加高光、景深、环境光遮蔽（SSAO）、调色等效果
        postEffect: {
          enable: true,
          bloom: {
            enable: false, // 高光特效
          },
          //环境光遮蔽
          SSAO: {
            enable: true,
            quality: 'medium', //遮蔽的质量
            radius: 10,
            intensity: 1.2,
          },
          //景深效果 景深效果是模拟摄像机的光学成像效果，在对焦的区域相对清晰，离对焦的区域越远则会逐渐模糊。
          depthOfField: {
            enable: false,
            focalRange: 5,
            fstop: 1,
            blurRadius: 6,
          },
        },
        // 地面
        groundPlane: {
          show: true,
          color: '#333',
        },
        // 、、 光照相关的设置
        light: {
          // 场景主光源
          main: {
            intensity: 6, //主光源的强度
            shadow: true, // 主光源是否投射阴影
            shadowQuality: 'high',
            alpha: 30, //主光源绕 x 轴，即上下旋转的角度
          },
          //全局的环境光设置
          ambient: {
            intensity: 0, //环境光的强度
          },
          //使用纹理作为环境光的光源
          ambientCubemap: {
            // texture: 'https://echarts.apache.org/examples/data-gl/asset/canyon.hdr',
            exposure: 2,
            diffuseIntensity: 1,
            specularIntensity: 1,
          },
        },
        viewControl: {
          minBeta: -360,
          maxBeta: 360,
        },
        itemStyle: {
          areaColor: '#666',
        },
        label: {
          color: 'white',
        },
        silent: true,
        instancing: true,
        boxWidth: 200,
        boxHeight: 1,
        data: regions,
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
  width: 100%;
  height: 100%;
}
</style>
