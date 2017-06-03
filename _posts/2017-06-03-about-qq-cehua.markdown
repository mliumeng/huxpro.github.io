---
layout:     post
title:      "仿QQ侧拉菜单笔记"
subtitle:   "回忆篇 来自2015-09-23 15:24。感谢当年的努力，继续努力一定会有质的飞跃"
date:       2017-06-03 9:14:00
author:     "刘蒙"
header-mask: 0.3
catalog:    true
tags:
    - android
    - 侧滑
    - qq
    - 自定义View
---

> 回忆篇 2015-09-23 15:24 迁移自我的CSDN
# 仿QQ侧拉菜单笔记
**今天模仿视频上的教程一步一步完成一个类似QQ侧滑菜单的一个demo再次留下笔记，以加强记忆。同时希望看到的各位多加批评多提建议（另外第一次用md排版，不好的话不要见怪)！**
## 应用场景
* 扩展主面板内容
*  炫酷菜单展示
## 实施步骤
- **分析QQ侧滑是有几部分构成**
- **分析每一部分都有什么功能与动画效果**
-  **分析完后就可以开始敲代码了**
## 分析结果
:  QQ侧滑效果是由三部分组成 ：
>  1. 背景图（BackGround）
>  2. 主面板（MainContent）
>  3. 左面版（LeftContent）

:  每一部分都有什么功能与动画效果 
> 1. 背景图（BackGround）  
> : 由暗到亮
> 
> 2. 主面板（MainContent） 
> : 缩小并且平移到屏幕的右侧
> 
> 3. 左面版（LeftContent）
> : 由屏幕左侧以外放大并且平移到屏幕中间

## 自定义View代码解说：

```xml
<!-- 上代码解说：布局文件 -->
<?xml version="1.0" encoding="utf-8"?>
<!-- com.vdh.view.DragLayout 这个引用自定义的view一定是包名加类名 -->
<com.vdh.view.DragLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@drawable/bg" 
    android:orientation="vertical" >
    
    <!-- 左面版 -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="#55aaaa"
        android:orientation="vertical" >

    <!-- 右面版 -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="#ffaaaa"
        android:orientation="vertical" >
    </LinearLayout>

</com.vdh.view.DragLayout>
<!-- 自定义的布局看出由三部分组成，与分析的QQ侧滑一致 -->
```
### 开始自定义view编写
>编写前 :
  >> 自定义的DragLayout继承于FrameLayout的原因：
  > 1. 首先因为FrameLayout是继承于GroupView自定义的View有两个视图所以必须继承于GroupView。
  > 2 .Framelayout的子View只有层级关系，不同与其他Layout，不用实现onMeasure(); onLayout();等。

> 实现：
>> ViewDragHelper : Google2013年IO大会提出，解决界面控件拖拽问题。（v4包下）。

#### 开始上代码
下面就通过注释一点一点讲解
```java
package com.vdh.view;

import com.nineoldandroids.view.ViewHelper;

import android.annotation.SuppressLint;
import android.content.Context;
import android.support.v4.view.ViewCompat;
import android.support.v4.widget.ViewDragHelper;
import android.support.v4.widget.ViewDragHelper.Callback;
import android.util.AttributeSet;
import android.util.Log;
import android.view.MotionEvent;
import android.view.View;
import android.view.ViewGroup;
import android.widget.FrameLayout;

@SuppressLint("ClickableViewAccessibility")
public class DragLayout extends FrameLayout {
	private ViewDragHelper mDragHelper; // 控件拖拽帮助类（v4 包下）
	private ViewGroup mLeftContent; // 左视图
	private ViewGroup mMainContent; // 主视图
	protected String tag = "DragLayout"; // log的TAG
	private int mHeight; // 屏幕高度
	private int mWidth; // 屏幕宽度
	private int mRange;  // 主视图能拖拽的最远位置
	/*实现三个方便用户各种场景下调用构造方法*/
	public DragLayout(Context context) {
		this(context, null);
	}

	public DragLayout(Context context, AttributeSet attrs) {
		this(context, attrs, 0);
	}

	public DragLayout(Context context, AttributeSet attrs, int defStyle) {
		super(context, attrs, defStyle);
		// 初始化操作（通过静态） 
		mDragHelper = ViewDragHelper.create(this, mCallback);
		// 初始化 mDragHelper  = ViewDragHelper.create(this,mCallback);这里注意一定是通过create初始化操作
	}

	ViewDragHelper.Callback mCallback = new Callback() {
		// 根据返回结果决定当前child是否可以拖拽
		// child 当前拖拽的子View
		// pointerId 区分多点触摸的手指ID（一般不用）
		@Override
		public boolean tryCaptureView(View child, int pointerId) {
			Log.d(tag, "tryCaptureView");
			// 这个方法关键看return 
			// return true 表示子view都可以拖拽
			// return false 表示所有子View都不会被拖拽
			// return child == mMainContent 表示mMainContent 可以被拖拽
			// return child == mLeftContent 表示mLeftContent 可以被拖拽
			return true; 
		}

		@Override
		public void onViewCaptured(View capturedChild, int activePointerId) {
			Log.d(tag, "onViewCaptured");
			// 当Captruechild被捕获时调用
			super.onViewCaptured(capturedChild, activePointerId);
		}

		// 根据建议值修正将要移动的位置（重要） 此时没有发生真正的移动 
		public int clampViewPositionHorizontal(View child, int left, int dx) {
			// child： 当前拖拽的View left： 新的位置的建议 dx：变化量
			if (child == mMainContent) {
				left = fixLeft(left); // 修正left值保证mMainContent在目标区域内滑动
			}
			return left;
		}

		@Override
		public int getViewHorizontalDragRange(View child) {
			// 返回拖拽的范围，不对拖拽进行真正的限制、getViewHorizontalDragRange仅仅决定了了动画执行速度
			return super.getViewHorizontalDragRange(child);
		}
		// 修正left值
		public int fixLeft(int left) {
			if (left < 0) {
				return 0;
			} else if (left > mRange) {
				return mRange;
			}
			return left;
		}

		// 当view位置改变的时候，处理重要的事情（更新状态，伴随动画，重绘界面）
		@Override
		public void onViewPositionChanged(View changedView, int left, int top,
				int dx, int dy) {
			// TODO Auto-generated method stub
			super.onViewPositionChanged(changedView, left, top, dx, dy);
			Log.d(tag, "onViewPositionChanged:" + " dx: " + dx + " left: "
					+ left);
			int newleft = left;
			if (changedView == mLeftContent) {
				newleft = mMainContent.getLeft() + dx;
				newleft = fixLeft(newleft);
				mLeftContent.layout(0, 0, mWidth, mHeight);
				mMainContent.layout(newleft, 0, mWidth + newleft, mHeight);
			}
			dispatchDragEvent(newleft); // 根据newleft执行动画
			invalidate();// 为了兼容低版本要进行重绘
		}

		// 手指松开调用该回调
		@Override
		public void onViewReleased(View releasedChild, float xvel, float yvel) {
			// releasedChild 要松开的子View
			// xvel x轴移动方向 x > 0 向右移动 x < 0 向左移动 x = 0 不移动
			// yvel y轴移动方向 同上x
			super.onViewReleased(releasedChild, xvel, yvel);
			// 在此考虑完open的可能后else的都是close的事件
			if (xvel == 0 && mMainContent.getLeft() > mRange / 2.0f) {
				openMainView();
			} else if (xvel > 0) {
				openMainView();
			} else {
				closeMainView();
			}
		}

		@Override
		public void onViewDragStateChanged(int state) {
			// TODO Auto-generated method stub
			super.onViewDragStateChanged(state);
		}

	};
	/*此方法是View屏幕测量完成后调用，用于获取屏幕的高度和宽度*/
	protected void onSizeChanged(int w, int h, int oldw, int oldh) {
		super.onSizeChanged(w, h, oldw, oldh);
		mHeight = getMeasuredHeight();
		mWidth = getMeasuredWidth();
		mRange = (int) (mWidth * 0.6f);
	}

	/**
	 * 更新状态执行动画
	 * 
	 * @param newleft
	 */
	protected void dispatchDragEvent(int newleft) {
		float percent = newleft * 1.0f / mRange;
		// 左面版动画
		ViewHelper.setScaleX(mLeftContent, evaluate(percent, 0.5f, 1.0f));
		ViewHelper.setScaleY(mLeftContent, 0.5f * percent + 0.5f);
		// -wigth/2.0f >>> 0.0f 平移动画
		ViewHelper.setTranslationX(mLeftContent,
				evaluate(percent, -mWidth / 2.0f, 0));
		ViewHelper.setAlpha(mLeftContent, evaluate(percent, 0.5f, 1.0f));
		// 主面板缩小动画
		ViewHelper.setScaleX(mMainContent, evaluate(percent, 1.0f, 0.8f));
		ViewHelper.setScaleY(mMainContent, evaluate(percent, 1.0f, 0.8f));
		// ViewHelper.setTranslationX(mMainContent, evaluate(percent, 0.0f,
		// 0.8f));

	}

	public Float evaluate(float fraction, Number startValue, Number endValue) {
		float startFloat = startValue.floatValue();
		return startFloat + fraction * (endValue.floatValue() - startFloat);
		// 这里自行画图理解,不祥述
	}

	protected void openMainView() {
		openMainView(true); // 默认平滑打开
	}

	protected void closeMainView() {
		closeMainView(true); // 默认平滑关闭
	}

	@Override
	public void computeScroll() {
		super.computeScroll();
	 // 
		if (mDragHelper.continueSettling(true)) {
			ViewCompat.postInvalidateOnAnimation(this);
		}
	}

	/**
	 * 关闭MainView
	 */
	public void closeMainView(boolean isSmooth) {
		int finalLeft = 0;
		if (isSmooth) {
			if (mDragHelper.smoothSlideViewTo(mMainContent, finalLeft, 0)) {
				ViewCompat.postInvalidateOnAnimation(this);
			}
		} else {
			mMainContent.layout(finalLeft, 0, finalLeft + mWidth, 0 + mHeight);
		}
	}

	/**
	 * 打开MainView
	 */
	public void openMainView(boolean isSmooth) {
		int finalLeft = mRange;
		if (isSmooth) {
			if (mDragHelper.smoothSlideViewTo(mMainContent, finalLeft, 0)) {
				ViewCompat.postInvalidateOnAnimation(this);
			}
		} else {
			mMainContent.layout(finalLeft, 0, finalLeft + mWidth, 0 + mHeight);
		}
	}

	// 传递事件（
	public boolean onInterceptTouchEvent(android.view.MotionEvent ev) {
		// 判断是否需要拦截事件
		return mDragHelper.shouldInterceptTouchEvent(ev);
	}

	@Override
	public boolean onTouchEvent(MotionEvent event) {
		try {
			mDragHelper.processTouchEvent(event);// 处理touch事件
		} catch (Exception e) {
			e.printStackTrace();
		}
		return true; // return true, 可以持续接受事件
	}

	@Override
	protected void onFinishInflate() {
		super.onFinishInflate();
		// GitHub 写注释 容错性检查（至少有两个子view，子view必须是viewGroup的子类）
		if (getChildCount() < 0) {
			throw new IllegalStateException(
					"your ViewGroup must have 2 childern at lest.");
		}
		if (!(getChildAt(0) instanceof ViewGroup && getChildAt(1) instanceof ViewGroup)) {
			throw new IllegalArgumentException(
					"your childer must be instance of ViewGroup.");
		}
		mLeftContent = (ViewGroup) getChildAt(0);
		mMainContent = (ViewGroup) getChildAt(1);
	}

}

```
#### 最后最重要的一步，就是在MainActivity引用定义好的布局

```
package com.vdh.activity;

import com.example.viewdraghelpertest.R;

import android.app.Activity;
import android.os.Bundle;

public class Main extends Activity {
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		// TODO Auto-generated method stub
		super.onCreate(savedInstanceState);
		setContentView(R.layout.drag_layout);
	}
}

```
#### the end 

> THANKS FOR YOUR TIME

---
>这篇博客来自2015-09-23 15:24 一晃两年过去了，曾经摸索开发，开发的路上迷茫不知所措，还好我没放弃。加油！这是一篇给我很多鼓励的博客。
> 有句话说的没错，以后你会感激你现在的努力 2017-06-03留