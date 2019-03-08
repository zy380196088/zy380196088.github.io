---
title: three.js 学习笔记
date: 2019-02-15 15:56:17
tags: 
categories: [ three.js ]
---

# 常用的object对象

| 对象名 | 介绍 |
| ------- | -----|
| plane | 平面:作为地面的二维矩形.渲染结果是平中央的一块黑色矩形 |
| cube | 方块:三维立方体|
| sphere | 球体 |
| wireframe | 线框 |
| camera | 相机 |
| Axes | 轴: x,y,z 辅助线 |
| scene | 场景 |
| rendered | 渲染器 |
| material | 材质 |
| stats | 状态:FPS帧频 |
| mesh | 网格 |
| Geometry | 几何体 |

# 常用方法

| 方法名 | 介绍 |
| ---- | ---- |
| requestAnimationFrame() | 用于执行renderer的动画操作 |
| setClearColorHex() | 高速rendere的背景色 |
| setSize() | 将scene 渲染成多大尺寸 |
| lookAt() | 使camera看向什么地方 |
| stats.update() | 刷新 FPS 帧频 |

<!--more-->

# 场景

## 创建场景

```js
var scene = new THREE.Scene();
```

### 场景加雾化效果

```js
scene.fog = new THREE.Fog(Oxffffff);
```

### 场景重要的属性和方法

| 函数/属性 | 描述 |
| ------- | -----|
| add(object) | 在场景中添加对象 |
| children | 返回一个场景中所有对象的列表,包括相机和光源|
| getChildByName(name) | 创建对象时,可以通过name属性为它指定一个独一无二的名字. |
| remove(object) | 从场景中删除它 |
| traverse(function) | children属性返回场景中的所有子对象的列表.通过traverse()函数可以传入一个回调函数访问这些子对象 |
| fog | 场景的雾化效果 |
| overrideMaterial | 强制场景中的所有物体都使用相同的材质 |

### 几何和网格对象

## 网格的属性和函数

| 函数/属性 | 描述 |
|----- | ----- |
| position | 位置:决定该对象相对其父对象的位置 |
| rotation | 旋转 |
| scale | 比例 |
| translateX() | X轴平移 |
| translateY() | Y轴平移 |
| translateZ() | Z轴平移 |

## 相机

### 正投影相机

### 透视相机

# 光源

## AmbientLight 环境光

基础光源,它的颜色会添加到整个场景和所有对象的当前颜色上;没有特定的光源,不会影响阴影;###不能将AmbientLight作为场景中唯一的光源;####

## PointLight 点光源

空间中的一点,朝所有的方向发射光线.

|属性|描述|
|----|----|
|color|颜色|
|intensity| 光照强度,默认值是1|
|distance | 距离 |
|position | 光源所在的位置 |
|visible| 是否可见|

```js
var poingLight = new THREE.PointLight(color);
```

## SpotLight 聚光灯光源

有聚光的效果的光源,类似台灯,吊灯,手电筒.

|属性|描述|
|----|----|
|castShadow|投影:设置为true,光源产生阴影|
|shadowCameraNear|投影近点|
|shadowCameraFar|投影远点|
|shadowCameraFov|投影视场:生成阴影的视场有多大|
|target |目标:决定光照的方向|
|shadowBias|阴影偏移:偏置阴影的位置|
|angle|角度|
|exponent|光强衰减指数|
|onlyShadow|仅阴影|
|shadowCameraVisible|投影方式可见|
|shadowDarkness|阴影暗度|
|shadowMapWidth|阴影映射宽度|
|shadowMapHeight|阴影映射高度|

## DirectionalLight 方向光

无限光,平行的光线,例如太阳光

## HemispherreLight 半球光

特殊光源,创建更加自然的室外光线,模拟反光面和光线微弱的天空
的方向发射光线

|属性|描述|
|----|----|
|groundColor|从地面发出光纤的颜色|
|Color|从天空发出光纤的颜色|
|intensity |光线照射的强度|

## AreaLight 面光源

散发光线的平面,而不是点散发的光线

## LensFlare 镜头眩光

添加眩光效果;

```js
var lensflare = THREE.LensFlare (texture,size,distance,bleding,color);
```

|属性|描述|
|----|----|
|texture|纹理|
|size|尺寸|
|distance|距离|
|blending|融合|
|color|颜色|

# 材质

## 材质分类

### MeshBasicMatreial 网格基础材质

### MeshDepthMaterial 网格深度材质

### MeshNormalMaterial 网格法向材质

### MeshFaceMaterial 网格面材质

### MeshLamberMaterial 网格朗伯材质

### MeshPhongMaterial 网格Phong式材质

### ShaderMaterial 着色器材质

### LineBasicMaterial 直线基础材质

### LineDashedMaterial 虚线材质

## 材质的基础属性

|属性|描述|
|----|----|
|ID|标识符|
|name |名称|
|opacity|透明度|
|transparent|是否透明|
|overdraw|过渡描绘|
|visible|是否可见|
|side|侧面|
|needsUpdate|是否刷新|

## 材质的融合属性

|属性|描述|
|----|----|
|blending|融合|
|blendsrc|融合源|
|blenddst|融合目标|
|blendingequation|融合公式|

## 高级属性

|属性|描述|
|----|----|
|depthTest|深度测试|
|depthWrite||
|polygonOffset,polygonOffset-Factor,polygonOffsetUnits|
|alphaTest|设定该属性为0-1的值,像素的alpha小于该值则不会显示出来|

# 几何体

# 高级几何体和二元操作

# 粒子系统

# 创建动画和移动相机

# 加载和使用纹理
