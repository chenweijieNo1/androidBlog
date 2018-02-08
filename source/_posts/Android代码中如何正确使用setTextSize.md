---
title: android 代码中如何正确使用setTextSize
date: 2018-1-24 15:17
categories: android常见问题
tags: [android常见问题]
---
有时我们会在代码中设置文字的大小，通常会使用mText.setTextSize(getResources().getDimension(R.dimen.font1)); （R.dimen.font1是我们要设置的文字的大小），但我们会发现文字会比我们想要的效果大很多，这是什么原因呢？

<dimen name="font1">18sp</dimen>

mText.setTextSize(18);  // 方法1
mText.setTextSize(getResources().getDimension(R.dimen.font1));  // 方法2
mText.setTextSize(TypedValue.COMPLEX_UNIT_PX,getResources().getDimension(R.dimen.font1));  // 方法3
mText.setTextSize(TypedValue.COMPLEX_UNIT_SP,18);  // 方法4

问题1: 方法1和方法2设置的文字尺寸大小相同么？
问题2:方法3和方法4设置的文字尺寸大小相同么？
问题3:方法1和方法4设置的文字尺寸大小相同么？

<!-- more -->

首先我们看看setTextSize的源代码：
```
public void setTextSize(float size) {
    setTextSize(TypedValue.COMPLEX_UNIT_SP, size);
}

public void setTextSize(int unit, float size) {   
    Context c = getContext();    
    Resources r;   
    if (c == null)        
        r = Resources.getSystem();    
    else        
        r = c.getResources();

    setRawTextSize(TypedValue.applyDimension(unit, size, r.getDisplayMetrics()));
}
```

我们可以看到第一个方式会默认使用TypedValue.COMPLEX_UNIT_SP，而第二种方式会使用你传进来的类型。

接下来看看setRawTextSize的源代码：
```
private void setRawTextSize(float size) {    
    if (size != mTextPaint.getTextSize()) {            
        mTextPaint.setTextSize(size);        
        if (mLayout != null) {            
            nullLayouts();            
            requestLayout();            
            invalidate();        
        }    
    }
}
```

TypedValue类中的applyDimension(...)方法
```
public static float applyDimension(int unit, float value, DisplayMetrics metrics){
    switch (unit) {    
        case COMPLEX_UNIT_PX:        
            return value;    
        case COMPLEX_UNIT_DIP:        
            return value * metrics.density;    
        case COMPLEX_UNIT_SP:        
            return value * metrics.scaledDensity;    
        case COMPLEX_UNIT_PT:        
            return value * metrics.xdpi * (1.0f/72);    
        case COMPLEX_UNIT_IN:        
            return value * metrics.xdpi;    
        case COMPLEX_UNIT_MM:        
            return value * metrics.xdpi * (1.0f/25.4f);    
    }    
    return 0;
}
```
如果传进来的是COMPLEX_UNIT_PX会直接返回；如果传入的unit为COMPLEX_UNIT_SP，则会将value处理成px返回

好了，回到开篇提到的四个问题，可以得出以下结论：

方法1：文字尺寸以sp为单位，大小为18
方法2：文字尺寸以sp为单位，大小为（18sp转换为px的值）
方法3：文字尺寸以px为单位，大小为（18sp转换为px的值）
方法4：文字尺寸以sp为单位，大小为18

方法1=方法3=方法4!＝方法2
