---
title: 微信小程序开发系列之flex布局
date: 2018-01-03 15:15:39
tags: [微信小程序]
---
微信小程序页面布局方式采用的是flex布局，flex可以简单、完整、响应式的实现各种页面布局，flex布局提供了元素在容器中的对齐，方向及顺序，能够调整子元素在不同屏幕大小中能够用最合适的方法填充合适的空间。

flex布局的特点：
- 任意方向的伸缩，向左向右，向上向下
- 在样式层可以调换和重排顺序
- 主轴和侧轴配置方便
- 子元素的空间拉伸和填充
- 沿着容器对齐

display:block 指定为块内容器模式，总是使用新行开始显示，微信小程序的视图容器（view，scroll-view和swiper）默认都是display:block

display:flex 指定为行内容器模式，在一行内显示子元素，可以使用flex-wrap属性指定是否换行

两者的效果区别如下所示:
<!-- more -->
```
<view class="container1">
    <view class="item1">
        1
    </view>
    <view class="item1">
        2
    </view>
    <view class="item1">
        3
    </view>
    <view class="item1">
        4
    </view>
</view>

.container1{
    height: 100%;
    width:100%;
}
.item1{
    height:100rpx;
    width:100rpx;
    background-color:cyan;
    border: 1px solid #fff
}
```
当container1加上display:block; 效果如下所示：
{% asset_img 1.png %}

当container1加上display:flex; 效果如下所示:
{% asset_img 2.png %}






