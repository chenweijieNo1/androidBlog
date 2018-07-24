---
title: 微信小程序开发系列之image组件的4种缩放模式和9种裁剪模式
date: 2018-07-24 10:03
tags: [微信小程序]
---
原始图片：
http://img04.tooopen.com/images/20130712/tooopen_17270713.jpg

4种缩放模式
- scaleToFill  默认模式，不保持纵横比缩放图片，使图片的宽高完全拉伸至填满image元素，如图所示：

{% asset_img scaleToFill.png %}

<!-- more -->
- aspectFit 保持纵横比缩放图片，使图片的长边能完全显示出来，也就是说，可以完整地将图片显示出来，如图所示：

{% asset_img aspectFit.png %}

- aspectFill 保持纵横比缩放图片，只保证图片的短边能完全显示出来。也就是说，图片通常只在水平或垂直方向是完整的，另一个方向将会发生截取，如图所示：

{% asset_img aspectFill.png %}

- widthFix 宽度不变，高度自动变化，保持原图宽高比不变，如图所示：

{% asset_img widthFix.png %}

9种裁剪模式：

- top  不缩放图片，只显示图片的顶部区域；
- bottom 不缩放图片，只显示图片的底部区域；
- center 不缩放图片，只显示图片的中间区域；
- left 不缩放图片，只显示图片的左边区域；
- right 不缩放图片，只显示图片的右边区域；
- top left 不缩放图片，只显示图片的左上边区域；
- top right 不缩放图片，只显示图片的右上边区域；
- bottom left 不缩放图片，只显示图片的左下边区域；
- bottom right 不缩放图片，只显示图片的右下边区域；




