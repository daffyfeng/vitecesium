# Vue 3 + Vite

This template should help get you started developing with Vue 3 in Vite. The template uses Vue 3 `<script setup>` SFCs, check out the [script setup docs](https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup) to learn more.

## Recommended IDE Setup

- [VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=johnsoncodehk.volar)

[Cesium：加载百度地图](https://blog.csdn.net/KaiSarH/article/details/106242572)

[Cesium 中文教程](http://cesiumcn.org/getstart.html)

## 下载模型

[真香时刻！免费提取城市三维建筑模型到建模软件的若干种方法](https://www.bilibili.com/video/BV1ft41187uf?p=1)

第一种方式下载无高度数据
第二种方式ok，下载后sketchup打开，导出dae格式文件

## 转换为gltf数据

目前gltf和bgltf都可以通过dae格式的三维模型转换而来。其中gltf的转换工具可以在https://github.com/KhronosGroup/glTF/wiki/Converter-builds 获取----colladaTogltf.exe，其能完成.dae到.gltf三维模型格式的转换

COLLADA2GLTF-bin.exe -i "C:\Users\Administrator\Desktop\cadmapper-chengdu-sichuan-cn.dae" -o "D:\daffyfengCode\aaa.gltf"

## Cesium

加载三位模型数据方式

3Dtiles

geojson

gltf

    Cesium中目前支持gltf和bgltf两种格式，gltf和bgltf都可以通过dae格式的
    三维模型转换而来，这里使用的模型是经过3ds Max导出的dae格式数据，而3ds
    Max本身不支持导出dae格式数据，要达到预期结果，需添加OpenCollada插件；
    再通过使用colladaTogltf.exe转换工具转成gltf格式或bgltf格式