---
title: 关于SASS
categories: [学习]
date: 2017-11-08 14:12:24
tags: [SASS]
---

# SASS 常用
1. @mixin 混合宏
建议使用混合宏来创建相同的代码块.
优点:可传参.
缺点:容易造成代码冗余.

```CSS
@mixin position-center($width,$height){
    width:$width;
    height:$height;
    position:absolute;
    left:50%;
    top:50%;
    margin:(-($height)/2) 0 0 (-($width)/2);
}

.center{
    @include position-center(600px,300px)
}

```

2. @extend() 继承
如果你的代码块不需要专任何变量参数，而且有一个基类已在文件中存在，那么建议使用 继承。
不足：如果是类(.class)，不管有没有调用(@extend)，在编译的时候，都会生成对应的CSS。


3. 占坑符 %placeholder
占坑和继承基本类似，唯一不同的是，相同的生成CSS块并没有在类中存在，而是额外声明。
如果不调用已声明的占位符，将不会产生任何CSS。
如果在不同选择器调用占位符，那么编译出来CSS将会把相同的代码合并在一起。
不足：就是不能传参啦！个人觉得%placeholder优于@extend。

```CSS
%mt15{
    margin-top:15px;
}

%pt15{
    padding-top:15px;
}

.btn6{
    @extend %mt15;
    @extend %pt15;
}

```

## 建议: 如果你需要参数变量，使用mixin。否则，继承一个placehodler。

# 写个 Sass组件
1. 定义提示颜色
> 验证
> 错误
> 警告
> 信息

2. 基本样式
```CSS
%message{
    padding: .05rem;
    border-radius: .1rem;
    border:1px solid ;
}

@mixin message($color){
    @extend %message;
    color:$color;
    border-color:lighten($color,20%);
    background:lighten($color,40%);
}
```

3. 调用 mixin
```CSS
.message-error {
  @include message(#b94a48);
}

.message-valid {
  @include message(#468847);
}

.message-warning {
  @include message(#c09853);
}

.message-info {
  @include message(#3a87ad);
}
```

4. 用嵌套列表（Nested Lists 可扩展配置的组件
```CSS
$message-types: (
  (error    #b94a48)
  (valid    #468847)
  (warning  #c09853)
  (info     #3a87ad)
) !default;

@each $message-type in $message-types {
  $type:  nth($message-type, 1);
  $color: nth($message-type, 2);

  .message-#{$type} {
    @include message($color);
  }
}
```

5. 使用 map (Sass 3.3)
可以使用Sass 3.3中新增的数据类型：Maps.代码更简洁方便.
```CSS
$message-types: (
  error   :  #b94a48,
  valid   :  #468847,
  warning :  #c09853,
  info    :  #3a87ad
) !default;

@each $type, $color in $message-types {
  .message-#{$type} {
    @include message($color);
  }
}
```


