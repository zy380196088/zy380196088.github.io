---
title: Mac VScode 更新错误
date: 2019-06-05 09:35:25
tags:
categories:
---

更新报错如下:
Could not create temporary directory: 权限被拒绝

解决方式:
终端下执行

```shell
sudo chown $USER ~/Library/Caches/com.microsoft.VSCode.ShipIt/  
```

<!--more-->
