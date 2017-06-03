---
layout:     post
title:      "[Nice UI] Material Design Library （Buttons）"
subtitle:   "Nice UI系列博客，持续更新中..."
date:       2017-05-29 10:40:00
author:     "刘蒙"
header-img: "img/button_header.jpg"
header-mask: 0.3
catalog:    true
tags:
    - android
    - videobutton
    - button
    - Material
---
> 本篇迁移自我的CSDN   
> 本篇持续在此更新...

#开始之前
> 朋友分享了一个GitHub上关于Android UI控件的集合站点。无比的炫酷与精彩。于是我决定对其进行一一剖析。目的，学习人家的技术。同时在此分享，记录
> 在此，坑已挖好。不管花费多少时间，希望我能坚持到最后
> 
> **此篇不定时持续更新**

- [传送门：Github UI集合](https://github.com/wasabeef/awesome-android-ui) 。
- 这里将一个一个对其进行剖析、模仿。
- **这里我们主要是学习自定义控件**
- 希望你们静下来慢慢看，内容有些漫长

---
##剖析内容
>- 这篇内容为Nice UI开篇之作。按照GitHub上面的顺序，我们首先讲解的是
[Material Design Android Library](https://github.com/navasmdc/MaterialDesignLibrary) 

Material Design 是专为设计适用于多个平台和设备的视觉、运动与互动效果而制定的综合指南。 Android 现在已支持 Material Design 应用。但是官方提供的support 并不能很好的向下兼容。于是就有不甘寂寞的大牛写下不朽Material Design Library 做到了无压力兼容（吹牛）。

![Material Design](https://raw.githubusercontent.com/navasmdc/MaterialDesignLibrary/master/images/logo.png)

###1.Button

|  Flat Button |Rectangle Button |Small Float Button|  Icon Button |Float Button   |
| :------------: | :------------: |:------------:| :------------: | :------------: |
| ![Flat Button](https://raw.githubusercontent.com/navasmdc/MaterialDesignLibrary/master/images/flat_button.png)  |  ![Rectangle Button](https://raw.githubusercontent.com/navasmdc/MaterialDesignLibrary/master/images/rectangle_button.png) |![Small Float Buttonn](https://raw.githubusercontent.com/navasmdc/MaterialDesignLibrary/master/images/float_small_button.png) |![Icon Buttonn](http://img.blog.csdn.net/20170119163430594?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbF9pdW1lbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast) |![Float Button](https://raw.githubusercontent.com/navasmdc/MaterialDesignLibrary/master/images/float_button.png) |


---
>Material Design 里面按钮一般 有以上几种形式。下面我们来看看他是怎么定义这几个按钮的。

![button](http://img.blog.csdn.net/20170119172400740?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbF9pdW1lbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
<p style="color:#ff0000;">根据源码：所有的以上5中按钮都是继承与Button，而Button继承于CustomView,然后几乎所有的控件都继承与CustomView 如图↓</p>
![CustomView](http://img.blog.csdn.net/20170119172843990?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbF9pdW1lbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

所以在剖析Button之前，首先要看一下CustomView，CustomeView是继承与RelativeLayout。往下看源码↓

-  **CustomeView**
```
package com.gc.materialdesign.views;

import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Color;
import android.util.AttributeSet;
import android.widget.RelativeLayout;

public class CustomView extends RelativeLayout{
    
    
    final static String MATERIALDESIGNXML = "http://schemas.android.com/apk/res-auto";
    final static String ANDROIDXML = "http://schemas.android.com/apk/res/android";
    
    final int disabledBackgroundColor = Color.parseColor("#E2E2E2");
    int beforeBackground;
    
    // Indicate if user touched this view the last time
    public boolean isLastTouch = false;

    public CustomView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }
    
    @Override
    public void setEnabled(boolean enabled) {
        super.setEnabled(enabled);
        if(enabled)
            setBackgroundColor(beforeBackground);
        else
            setBackgroundColor(disabledBackgroundColor);
        invalidate();
    }
    
    boolean animation = false;
    
    @Override
    protected void onAnimationStart() {
        super.onAnimationStart();
        animation = true;
    }
    
    @Override
    protected void onAnimationEnd() {
        super.onAnimationEnd();
        animation = false;
    }
    
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        if(animation)
            invalidate();
    }
}

```
这段代码很简单，仅仅是规定了一些基础的状态。因为都是继承与RelativeLayout 所以我们在setEnable()的时候背景颜色不会发生改变，所以在CustomView 里我们对setEnable()进行处理。setEnable的同时改变其背景颜色。同时对动画的绘制进行状态控制。简单讲完CustomView再开始剖析Button

- **Button 关键代码 （一） 记录坐标以及状态**
```

 /**
  * onTouchEvent
  * 先读代码
  * */ 
 @Override
    public boolean onTouchEvent(MotionEvent event) {
        invalidate();
        if (isEnabled()) {
            isLastTouch = true;
            isActionUp = false;
            if (event.getAction() == MotionEvent.ACTION_DOWN) {
                radius = getHeight() / rippleSize;//将高度的1/rippleSize
                x = event.getX();//获取点击处的坐标
                y = event.getY();
                beforeClickTextColor = textColor;//记录点击前字体颜色
                setTextColor(makePressColor(textColor));
            } else if (event.getAction() == MotionEvent.ACTION_MOVE) {
                radius = getHeight() / rippleSize;
                x = event.getX();
                y = event.getY();
                if (!((event.getX() <= getWidth() && event.getX() >= 0) && (event
                        .getY() <= getHeight() && event.getY() >= 0))) {
                    // 当手指移出button
                    setTextColor(beforeClickTextColor);
                    isLastTouch = false;
                    x = -1;
                    y = -1;
                }
            } else if (event.getAction() == MotionEvent.ACTION_UP) {
                setTextColor(beforeClickTextColor);
                isActionUp = true;
                if (((event.getX() <= getWidth() && event.getX() >= 0) && (event
                        .getY() <= getHeight() && event.getY() >= 0))) {
                    radius++;
                    if (!clickAfterRipple && onClickListener != null) {
                        onClickListener.onClick(this);
                    }
                } else {
                    // 当手指移出button
                    isLastTouch = false;
                    x = -1;
                    y = -1;
                }
            } else if (event.getAction() == MotionEvent.ACTION_CANCEL) {
                isLastTouch = false;
                x = -1;
                y = -1;
            } 
        }
        return true;
    }
```
> 上面这端代码其主要目的是记录手指点击的位置（坐标），点击的状态以及初始半径

- **Button 关键代码 （二） 画圆**
```
    public Bitmap makeCircle() {
        int i = getWidth() * getWidth() + getHeight() * getHeight();
        // 勾股定理 Math.sprt(int i)开根号
        double maxLine = Math.sqrt(i) + 10;

        Bitmap output = Bitmap.createBitmap(
                getWidth() - Utils.dpToPx(6, getResources()), getHeight()
                        - Utils.dpToPx(7, getResources()), Bitmap.Config.ARGB_8888);// 创建一个这么大的bitmap
        Canvas canvas = new Canvas(output);// 创建一个bitmap画布
        canvas.drawARGB(0, 0, 0, 0);
        Paint paint = new Paint();
        paint.setAntiAlias(true);
        paint.setColor(rippleColor);
        canvas.drawCircle(x, y, radius, paint);
        if (radius >= getHeight() / rippleSize)
            radius += rippleSpeed;
        if (radius >= maxLine) {
            x = -1;
            y = -1;
            radius = (float) maxLine;
            if (isActionUp && onClickListener != null && clickAfterRipple)
                onClickListener.onClick(this);
        }
        return output;
    }
```
> 这里就是画圈,代码很简单，就是在button点击的坐标上移radius位半径画圆

- **Button 不太关键代码 （三） 对当前颜色加深 （eg:蓝色转深蓝色）**
 

```
  protected int makePressColor(int makeColor) {
        int r = (makeColor >> 16) & 0xFF; 
        int g = (makeColor >> 8) & 0xFF;
        int b = (makeColor >> 0) & 0xFF;
        r = (r - 30 < 0) ? 0 : r - 30;
        g = (g - 30 < 0) ? 0 : g - 30;
        b = (b - 30 < 0) ? 0 : b - 30;
        return Color.rgb(r, g, b);
    }
```

    
 
 
 至此，所有button的基类Button就这么几个关键点，切通俗易懂。不再赘述
#### **Flat Button**    
首先我们来实现第一个button ![Flat Button](https://raw.githubusercontent.com/navasmdc/MaterialDesignLibrary/master/images/flat_button.png)← 就是这个button
1. 关键代码 onDraw()
 > onDraw() 的逻辑其实很简单 就是不断的

```

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        if (x != -1) {
            Paint paint = new Paint();
            paint.setAntiAlias(true);
            paint.setColor(rippleColor);
            canvas.drawCircle(x, y, radius, paint);
            if (radius > getHeight() / rippleSize)
                radius += rippleSpeed;
            if (radius >= getWidth()) {
                x = -1;
                y = -1;
                radius = getHeight() / rippleSize;
                if (onClickListener != null && clickAfterRipple)
                    onClickListener.onClick(this);
            }
            invalidate();// 刷新界面。执行此方法会再次执行onDraw
        }
    }
```
###**Rectangle Button**
> 发现Rectangle Button 与 Flat Button 除了background不同之外没有其他的不同。主要是背景加了阴影

1. RectangleButton 设置阴影关键代
- 
- 设置 background 

```
 setBackgroundResource(R.drawable.rectangle_button_bg);
```
- rectangle_button_bg.xml
```
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/background_button" />
    <item android:id="@+id/bg_sharp">
        <shape android:shape="rectangle">
            <solid android:color="#fff"/>
            <corners android:radius="2dp" />
        </shape>
    </item>
</layer-list>
```
- background_button.png.9

    <center>![background_button.png.9图](http://img.blog.csdn.net/20170420190335725?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbF9pdW1lbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)</center>

- 设置背景色（backgroundColor）
 

```
  public void mSetBackgroundColor(int color) {
        try {
            LayerDrawable layerDrawable = (LayerDrawable) getBackground();
            GradientDrawable gradientDrawable = (GradientDrawable) layerDrawable
                    .findDrawableByLayerId(R.id.bg_sharp);
            gradientDrawable.setColor(color);
        } catch (Exception e) {
            e.printStackTrace();
        }

    }
```

 



