---
title: android解决输入法键盘遮盖布局问题
date: 2017-12-15 15:12:55
categories: android常见问题
tags: [android常见问题]
---
代码如下所示：

```
/**
     * addLayoutListener方法如下
     * @param main 根布局
     * @param scroll 需要显示的最下方View
     */
public void addLayoutListener(final View main, final View scroll) {
        main.getViewTreeObserver().addOnGlobalLayoutListener(new ViewTreeObserver.OnGlobalLayoutListener() {
            @Override
            public void onGlobalLayout() {
                Rect rect = new Rect();
                //1、获取main在窗体的可视区域
                main.getWindowVisibleDisplayFrame(rect);
                //2、获取main在窗体的不可视区域高度，在键盘没有弹起时，main.getRootView().getHeight()调节度应该和rect.bottom高度一样
                int mainInvisibleHeight = main.getRootView().getHeight() - rect.bottom;
                int screenHeight = main.getRootView().getHeight();//屏幕高度
                RelativeLayout scrollView = (RelativeLayout) main.findViewById(R.id.input_panel);
                //3、不可见区域大于屏幕本身高度的1/4：说明键盘弹起了
                if (mainInvisibleHeight > screenHeight / 4) {
                    int[] location = new int[2];
                    scroll.getLocationInWindow(location);
                    // 4、获取Scroll的窗体坐标，算出main需要滚动的高度
                    int srollHeight = (location[1] + scroll.getHeight()) - rect.bottom;
                    //5、让界面整体上移键盘的高度
                    if (viewScrollReset) {
                        scrollView.scrollTo(0, srollHeight + 10);
                        viewScrollReset = false;
                    }
                } else {
                    //3、不可见区域小于屏幕高度1/4时,说明键盘隐藏了，把界面下移，移回到原有高度
                    scrollView.scrollTo(0, 0);
                    viewScrollReset = true;
                }
            }
        });
    }
```
