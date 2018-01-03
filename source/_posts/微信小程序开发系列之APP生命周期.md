---
title: 微信小程序开发系列之APP生命周期
date: 2017-12-28 15:00
categories: 微信小程序
tags: [微信小程序]
---
#### 1、微信小程序APP的生命周期方法
微信小程序启动时，调用的生命周期方法为：onLaunch->onShow->onLoad

当小程序置于后台时，系统回调生命周期方法：onHide

### 2、如何调用全局方法和变量
在app.js中可以自定义全局变量，如下：
```
myData:{
    username:"abc"
},
```
如果想在index.js中调用username，则如下所示：先获取全局app，再获取myData，再获取username
```
var app = getApp() //获取全局app
Page({
    data: {
        name: 'test'
    },
    test: function(){
        this.setData(name:app.myData.username) //通过app获取全局变量username
    }

    })
```
<!-- more -->
