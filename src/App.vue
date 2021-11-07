<template>
  <div id="cesiumContainer"></div>
</template>

<script setup>
import * as Cesium from 'cesium';
import { onMounted } from 'vue'
import BaiduImageryProvider from "./assets/js/baiduImageryProvider.js";

onMounted(() => {
  const viewer = new Cesium.Viewer("cesiumContainer", {
    animation : false,//是否创建动画小器件，左下角仪表    
    baseLayerPicker : false,//是否显示图层选择器    
    fullscreenButton : false,//是否显示全屏按钮    
    geocoder : false,//是否显示geocoder小器件，右上角查询按钮    
    homeButton : false,//是否显示Home按钮    
    infoBox : false,//是否显示信息框    
    sceneModePicker : false,//是否显示3D/2D选择器    
    selectionIndicator : false,//是否显示选取指示器组件    
    timeline : false,//是否显示时间轴    
    navigationHelpButton : false,//是否显示右上角的帮助按钮    
    scene3DOnly : true,//如果设置为true，则所有几何图形以3D模式绘制以节约GPU资源    
    clock : new Cesium.Clock(),//用于控制当前时间的时钟对象    
    selectedImageryProviderViewModel : undefined,//当前图像图层的显示模型，仅baseLayerPicker设为true有意义    
    imageryProviderViewModels : Cesium.createDefaultImageryProviderViewModels(),//可供BaseLayerPicker选择的图像图层ProviderViewModel数组    
    selectedTerrainProviderViewModel : undefined,//当前地形图层的显示模型，仅baseLayerPicker设为true有意义    
    terrainProviderViewModels : Cesium.createDefaultTerrainProviderViewModels(),//可供BaseLayerPicker选择的地形图层ProviderViewModel数组    
    imageryProvider : new BaiduImageryProvider({
        url: "http://online{s}.map.bdimg.com/onlinelabel/?qt=tile&x={x}&y={y}&z={z}&styles=pl&scaler=1&p=1"
    }), // 图像图层提供者，仅baseLayerPicker设为false有意义
    terrainProvider : new Cesium.EllipsoidTerrainProvider(),//地形图层提供者，仅baseLayerPicker设为false有意义    
    skyBox : new Cesium.SkyBox({    
        sources : {    
          positiveX : 'Cesium-1.7.1/Skybox/px.jpg',    
          negativeX : 'Cesium-1.7.1/Skybox/mx.jpg',    
          positiveY : 'Cesium-1.7.1/Skybox/py.jpg',    
          negativeY : 'Cesium-1.7.1/Skybox/my.jpg',    
          positiveZ : 'Cesium-1.7.1/Skybox/pz.jpg',    
          negativeZ : 'Cesium-1.7.1/Skybox/mz.jpg'    
        }    
    }),//用于渲染星空的SkyBox对象    
    fullscreenElement : document.body,//全屏时渲染的HTML元素,    
    useDefaultRenderLoop : true,// 如果需要控制渲染循环，则设为true    
    targetFrameRate : undefined,// 使用默认render loop时的帧率    
    showRenderLoopErrors : false,// 如果设为true，将在一个HTML面板中显示错误信息    
    automaticallyTrackDataSourceClocks : true,// 自动追踪最近添加的数据源的时钟设置    
    contextOptions : undefined,// 传递给Scene对象的上下文参数（scene.options）    
    sceneMode : Cesium.SceneMode.SCENE3D,// 初始场景模式    
    mapProjection : new Cesium.WebMercatorProjection(),// 地图投影体系    
    dataSources : new Cesium.DataSourceCollection()    //需要进行可视化的数据源的集合  
      
  })

  viewer.camera.setView({
        destination: Cesium.Cartesian3.fromDegrees(116.46, 39.92, 10000000.0),
    }); //  设置初始位置
  // 分辨率调整函数
  let adjustmentPixel = function () {
    // 判断是否支持图像渲染像素化处理
    var supportsImageRenderingPixelated = viewer.cesiumWidget._supportsImageRenderingPixelated;
    if (supportsImageRenderingPixelated) {
        // 直接拿到设备的像素比例因子 - 如我设置的1.25
        var vtxf_dpr = window.devicePixelRatio;
        // 这个while我们在后面会做一个说明，但不是解决问题的说明
        while (vtxf_dpr >= 2.0) {
            vtxf_dpr /= 2.0;
        }
        // 设置渲染分辨率的比例因子
        viewer.resolutionScale = vtxf_dpr;
    }
  };
  adjustmentPixel();
  viewer.scene.postProcessStages.fxaa.enabled = false;
  //viewer.scene.globe.depthTestAgainstTerrain = true;
  viewer.scene.fxaa = false;
  let handler = new Cesium.ScreenSpaceEventHandler(viewer.canvas);
  handler.setInputAction(function (event) {
      let earthPosition  = viewer.camera.pickEllipsoid(event.position,viewer.scene.globe.ellipsoid);
      if (Cesium.defined(earthPosition)) {
          let ellipsoid = viewer.scene.globe.ellipsoid;
          let cartographic = ellipsoid.cartesianToCartographic(earthPosition);
          let lat = Cesium.Math.toDegrees(cartographic.latitude);
          let lon = Cesium.Math.toDegrees(cartographic.longitude);
      }
  }, Cesium.ScreenSpaceEventType.LEFT_CLICK);
  

})
</script>



<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
html,body,cesiumContainer{
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
  overflow: hidden;
}
</style>
