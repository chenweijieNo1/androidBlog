---
title: ViewPager使用详解
date: 2017-12-21 14:11
categories: android控件使用
tags: [android控件使用]
---
#### 前言
在开发中ViewPager是不可或缺的控件，引导页、轮播图、卡片画廊等效果大多数是用ViewPager来实现，该文章主要是梳理下ViewPager的用法。
主要包括以下内容：
- ViewPager的简介与作用
- ViewPager的适配器
- 自定义ViewPager的切换效果
- ViewPager的基础使用
- ViewPager结合第三方库实现小圆点指示器效果
- ViewPager结合design库实现tab切换
- 基于ViewPager实现广告轮播控件

#### 1、ViewPager的简介与作用
ViewPager是android扩展包v4包中的类，继承自ViewGroup，它是一个容器类，可以添加其他的view，实现多个view左右切换、滑动，这个控件大家使用比较多，也比较熟悉了，下面说下常用的几个方法：
- setAdapter(PagerAdapter adapter) 设置适配器
- setOffscreenPageLimit(int limit) 设置缓存的页面个数，默认是1个
- setCurrentItem(int item) 跳转到某个页面
- setOnPageChangeListener(...) 设置页面滑动时的监听器
- setPageTransformer(...) 设置页面切换时的动画效果
- setPageMargin(int margin) 设置不同页面之间的间隔

### 2、ViewPager的适配器
ViewPager需要设置PagerAdapter来完成页面和数据的绑定，这个PagerAdapter是个基类，我们经常使用它的两个子类：FragmentPagerAdapter和FragmentStatePagerAdapter，先说说它们之间的使用区别吧。
<!-- more -->
PagerAdapter是个抽象的适配器，如果继承自该类，至少需要实现 instantiateItem(), destroyItem(), getCount() 以及 isViewFromObject()。
```
public class AdapterViewpager extends PagerAdapter {
    private List<View> mViewList;

    public AdapterViewpager(List<View> mViewList) {
        this.mViewList = mViewList;
    }

    @Override
    public int getCount() {//必须实现 数据个数
        return mViewList.size();
    }

    @Override
    public boolean isViewFromObject(View view, Object object) {//必须实现 这个方法用于判断是否由对象生成界面
        return view == object;
    }

    @Override
    public Object instantiateItem(ViewGroup container, int position) {//必须实现，实例化 要显示的页面或需要缓存的页面，进行布局的初始化
        container.addView(mViewList.get(position));
        return mViewList.get(position);
    }

    @Override
    public void destroyItem(ViewGroup container, int position, Object object) {//必须实现，销毁
        container.removeView(mViewList.get(position));
    }
}
```

FragmentPagerAdapter和FragmentStatePagerAdapter更专注于每一项是fragment的情况，FragmentPagerAdpater将每一个页面表示为一个fragment，并且每一个fragment都将会保存到FragmentManger当中，当用户没可能再次回到页面的时候，FragmentManager才会将这个fragment销毁，对于不再需要的fragment，选择调用onDetach()方法，仅销毁视图，但不会销毁fragment实例；FragmentStatePagerAdapter会销毁不再需要的fragment，当当前事务提交以后，会彻底的将fragment从当前activity的FragmentManger中移除，销毁时会将其onSaveInstance(Bundle outState)中Bundle保存下来，当用户切换回来，可以通过该bundle恢复生成新的fragment.
总的来说，FragmentStatePagerAdapter更省内存，但花时间，一般情况下，如果只有3、4个tab，可以选择FragmentPagerAdapter；如果展示数量特别多的条目，可以考虑FragmentStatePagerAdapter.

两者的实现方式一样，如下所示：

```
public class AdapterFragment extends FragmentPagerAdapter {
    private List<Fragment> mFragments;

    public AdapterFragment(FragmentManager fm, List<Fragment> mFragments) {
        super(fm);
        this.mFragments = mFragments;
    }

    @Override
    public Fragment getItem(int position) {//必须实现
        return mFragments.get(position);
    }

    @Override
    public int getCount() {//必须实现
        return mFragments.size();
    }

    @Override
    public CharSequence getPageTitle(int position) {//选择性实现
        return mFragments.get(position).getClass().getSimpleName();
    }
}
```

### 3、自定义ViewPager的切换效果
官方提供了一个内部接口ViewPager.PageTransformer来供我们实现自定义切换动效，这个接口只提供一个方法：public void transformPage(View view,float position)

transformPage有两个参数，一个是view，当前要设置动效的页面，这个页面不单单是当前显示的页面，即将滑出的页面，即将滑入的页面，已经隐藏的页面，但如何分辨View指哪个页面呢？这就需要第二个参数position来辨别。

从doc注释来看，当前选中的item的position为0，被选中item的前一个为-1，后一个为1，这前提是没有设置pageMargin，如果设置了pageMargin，前后item的position需要分别加上或减去（前减后加）一个偏移量（pageMargin / pageWidth）

滑动界面时postion是动态变化的，假设有三个页面view1，view2，view3从左至右在viewPager中显示：

- 往左滑动时：view1，view2，view3的position都是不断变小的。
  view1的position: 0 → -1 → 负无穷大
  view2的position: 1 → 0 → -1
  view3的position: 1 → 0

- 往右滑动时：view1，view2，view3的position都是不断变大的。
  view1的position: -1 → 0
  view2的position: -1 → 0 → 1
  view3的position: 0 → 1→ 正无穷大

当position是正负无穷大时view就离开屏幕视野了。因此最核心的控制逻辑是在[-1,0]和(0,1]这两个区间，通过设置透明度，平移，旋转，缩放等动画组合可以实现各式各样的页面变化效果。

```
public class ZoomOutPageTransformer implements ViewPager.PageTransformer {
  private static final float MIN_SCALE = 0.85f;
  private static final float MIN_ALPHA = 0.5f;

  @SuppressLint("NewApi")
  public void transformPage(View view, float position) {
      int pageWidth = view.getWidth();
      int pageHeight = view.getHeight();

      Log.e("TAG", view + " , " + position + "");

      if (position < -1) { // [-Infinity,-1)
          // This page is way off-screen to the left.
          view.setAlpha(0);

      } else if (position <= 1) 
      { // [-1,1]
          // Modify the default slide transition to shrink the page as well
          float scaleFactor = Math.max(MIN_SCALE, 1 - Math.abs(position));
          float vertMargin = pageHeight * (1 - scaleFactor) / 2;
          float horzMargin = pageWidth * (1 - scaleFactor) / 2;
          if (position < 0) {
              view.setTranslationX(horzMargin - vertMargin / 2);
          } else {
              view.setTranslationX(-horzMargin + vertMargin / 2);
          }

          // Scale the page down (between MIN_SCALE and 1)
          view.setScaleX(scaleFactor);
          view.setScaleY(scaleFactor);

          // Fade the page relative to its size.
          view.setAlpha(MIN_ALPHA + (scaleFactor - MIN_SCALE)
                  / (1 - MIN_SCALE) * (1 - MIN_ALPHA));

      } else { // (1,+Infinity]
          // This page is way off-screen to the right.
          view.setAlpha(0);
      }
  }
}
```

### 4、ViewPager的简单使用

- 布局文件声明控件
```
<!--填充整个页面的ViewPager-->
   <android.support.v4.view.ViewPager
      android:id="@+id/viewpager"
      android:layout_width="match_parent"
      android:layout_height="match_parent"/>
```
- 代码中设置数据
```
ViewPager viewPager = (ViewPager) findViewById(R.id.viewpager);
自定义类实现PagerAdapter，填充显示数据
viewPager.setAdapter(new MyAdapter());
```

### 5、ViewPager结合第三方库实现小圆点指示器效果

[https://github.com/ongakuer/CircleIndicator](https://github.com/ongakuer/CircleIndicator)

### 6、ViewPager结合design库实现tab切换

[http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0731/3247.html](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0731/3247.html)

### 7、基于ViewPager实现广告轮播控件

[https://github.com/daimajia/AndroidImageSlider](https://github.com/daimajia/AndroidImageSlider)








