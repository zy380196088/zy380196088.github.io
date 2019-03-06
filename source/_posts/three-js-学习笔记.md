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

# Three.js库提供的各种光源

## AmbientLight 环境光

基础光源,它的颜色会添加到整个场景和所有对象的当前颜色上

## PointLight 点光源

空间中的一点,朝所有的方向发射光线

## SpotLight 聚光灯光源

有聚光的效果的光源,类似台灯,吊灯,手电筒

## DirectionalLight 方向光

无限光,平行的光线,例如太阳光

## HemispherreLight 半球光

特殊光源,创建更加自然的室外光线,模拟反光面和光线微弱的天空

## AreaLight 面光源

散发光线的平面,而不是点散发的光线

## LensFlare 镜头眩光

添加眩光效果