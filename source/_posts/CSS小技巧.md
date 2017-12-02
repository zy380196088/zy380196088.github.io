---
title: CSS常用的
date: 2016-05-19 08:41:27
tags: [ CSS, CSS3 ]
---
## 经典中的经典 : 垂直居中的方法
### 弹性盒子布局flex
给父容器设置:
```CSS
display:flex;
justify-content:center;
align-items:center;
```
### 绝对定位居中
```CSS
.parent{
  position:relative;
}
.children{
  margin:auto;
  position:absolute;
  top:0;
  right:0;
  bottom:0;
  left:0;
}
```
### 负边距,变形
```CSS
.parent{
  width:100px;
  height:100px;
  position:relative;
}
.chiildren{
  width:50px;
  height:50px;
  position:absolute;
  top:50%;
  left:50%;
  transform:translate(-50%,-50%)
}
```

## CSS背景图拉伸自适应尺寸，全浏览器兼容

```CSS
.bg{
	background:url(http://wyz.67ge.com/wp-content/uploads/qzlogo.jpg);
	filter:"progid:DXImageTransform.Microsoft.AlphaImageLoader(sizingMethod='scale')";
	-moz-background-size:100% 100%;
	background-size:100% 100%;
}
```

## CSS模拟 placeholder

给元素设置contenteditable和data-placeholder，再利用:empty:before和:focus:before就可以模拟input placeholder了，focus时placeholder就消失，效果感人。

## 更改浏览器的滚动条样式

```CSS
body {
		scrollbar-arrow-color: red; /*上下按钮上三角箭头的颜色*/
		scrollbar-face-color: #CBCBCB; /*滚动条凸出部分的颜色*/
		scrollbar-3dlight-color: blue; /*滚动条亮边的颜色*/
		scrollbar-highlight-color: #333; /*滚动条空白部分的颜色*/
		scrollbar-shadow-color: yellow; /*滚动条阴影的颜色*/
		scrollbar-darkshadow-color: green; /*滚动条强阴影的颜色*/
		scrollbar-track-color: #eee; /*滚动条背景颜色*/
		scrollbar-base-color: black; /*滚动条的基本颜色*/
		Cursor:url(mouse.cur); /*自定义个性鼠标*/
		/*以上2项适用与：body、div、textarea、iframe*/
  }
      ::-webkit-scrollbar {  /*滚动条整体部分 */
          width:10px;
          margin-right:2px
      }
      ::-webkit-scrollbar-button { /*滚动条两端的按钮 */
          width:10px;
          background-color: yellow;
      }
      ::-webkit-scrollbar:horizontal {
          height:10px;
          margin-bottom:2px
      }
      ::-webkit-scrollbar-track {  /*外层轨道*/
          border-radius: 10px;
      }
      ::-webkit-scrollbar-track-piece {  /*内层轨道，滚动条中间部分*/
          background-color: #333333;
          border-radius: 10px;
      }
      ::-webkit-scrollbar-thumb {  /* 滑块 */
          width:10px;
          border-radius: 5px;
          background: #CBCBCB;
      }
      ::-webkit-scrollbar-corner { /* 边角 */
          width: 10px;
          background-color: red;
      }
      ::-webkit-scrollbar-thumb:hover { /* 鼠标移入滑块 */
          background: #909090;
      }
      .demo {
          width: 400px;
          height: 200px;
          overflow: auto;
      }
```

## 清除浮动

1. 添加额外标签
```CSS
.clearfix{
	clear:both;
}
```
2. 使用:after伪元素
给父元素加上:after伪元素
```CSS
.clearfix:after{
	content:'';
	display:block;
	clear:both;
}
.clearfix{/*兼容IE*/
	zoom:1;
}
```
3. 给父元素定高

4. 利用overflow:hidden
父元素样式加上overflow:hidden;

## 让页面上的内容不能被选中
```CSS
body{
    -webkit-user-select:none;
    -moz-user-select:none;
    -ms-user-select:none;
    user-select:none;
}
```

## 单行超出省略字
```CSS
div{
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
```

## iOS惯性滚动
```CSS
.div{
  -webkit-overflow-scrolling:touch !important;
}
```

## CSS3 之 blur (高斯模糊,毛玻璃效果)
```CSS
.blur{
    -webkit-filter: blur(10px); /* Chrome, Opera */
       -moz-filter: blur(10px);
        -ms-filter: blur(10px);
            filter: blur(10px);
}
```

注意:IE 不支持blur,IE6?-IE9浏览器可以借助IE filter模糊滤镜实现，如下CSS：
```CSS
filter: progid:DXImageTransform.Microsoft.Blur(PixelRadius=10, MakeShadow=false); 
```

## span 标签换行问题
加上样式：
```CSS
.break{
  display:inline-block;
  width:60%;
  word-wrap:break-word;
  white-space:normal;
}
```
有时候给span样式设置了 display:block;活着 display:inline-block;后仍然不换行...
是因为 span 不是块状元素。本身自带有 左浮动的效果，并且连续的英文字母跟数字是没办法 自动换行的;必须要**强制换行**。
但是光用word-wrap:break-word; 是不行的。所以,必须要在限制了宽度的情况下,还要增加 white-space:normal;
