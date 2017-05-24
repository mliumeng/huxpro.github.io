---
layout:     post
title:      "WebView整理"
subtitle:   "webView 使用总结。常用的控件却总是记不住使用方法。每次上网找代码。因为使用的简单所以总是容易忽略，在此记录"
date:       2017-05-04 17:00:00
author:     "liumeng"
header-img: "img/post-bg-nextgen-web-pwa.jpg"
header-mask: 0.3
catalog:    true
tags:
    - android
    - webview
---

# WebView 篇
## 前言
> webView 使用总结。常用的控件却总是记不住使用方法。每次上网找代码。因为使用的
> 简单所以总是容易忽略，在此记录

**此篇主要记录以下几个知识点，我认为可以覆盖我所需的功能需求<如有新的将持续更新>**
<center>
<table>
    <tr style="background-color: #aaa";>
        <th colspan="2">本篇WebView功能介绍点</th>
    </tr>
    <tr style="background-color: #ddd">
        <th>功能</th>
        <th>介绍</th>
    </tr>
    <tr>
        <td>加载网页</td>
        <td>1.加载URL 2.加载本地HTML</td>
    </tr>
        <tr>
        <td>获取网页信息</td>
        <td>获取title favicon video image 等资源</td>
    </tr>    <tr>
        <td>植入js</td>
        <td>控制网页：去掉title 去掉广告等等</td>
    </tr>
</table>
</center>   
## 一、 webview 基础用法：
> 我认为以下三点最为常用，可应付一般场景

<b>
<ul>
    <li> 加载网址</li>
    <li> 加载本地html</li>
    <li> 不跳转系统浏览器</li>
</ul>
</b>

### 1.1 加载网址

```java
String url = "http://www.baidu.com";
WebView webview = (WebView)findViewById(R.id.web);
webview.getSettings().setJavaScriptEnabled(true);//设置支持javaScriptEnabled,默认不支持

webview.load(url);
```

### 1.2 加载本地HTML

```java
String data = "<html>
                    <body>
                      <h1>这是标题一</h1>
                    </body>
                </html>";
WebView webview = (WebView)findViewById(R.id.web);
webview.loadData(data,"text/html","utf-8");
```

### 1.3 不跳转到系统浏览器

```java
     private class MyWebViewClient extends WebViewClient {
        @Override
        public void onLoadResource(WebView view, String url) {
            super.onLoadResource(view, url);
        }

        public boolean shouldOverrideUrlLoading(WebView view, String url) {
            view.loadUrl(url);
            return true;
        }
    }
String url = "http://www.baidu.com";
WebView webview = (WebView)findViewById(R.id.web);
webview.load(url);
webview.setWebViewClient(new MyWebViewClient());// 设置webViewClient 重写shouldOverrideUrlLoading(...)
```

## 二、 webview 进一步的用法：

> 不敢说进阶，怕被嘲笑。2233333...........

### 2.1 获取网页信息
> 获取网页信息，这里就要提到两个Client：WebViewClient和WebChromeClient
> 
> **区别**
> 
> * 1.WebViewClient主要帮助WebView处理各种通知、请求事件的.
> * 2.WebChromeClient主要辅助WebView处理Javascript的对话框、网站图标、网站title、加载进度等

> 我个人的理解：WebViewClient 
偏重于展示网页状态，而WebChromeClient则注重于控制网页 

<ul>
    <li>title</li> 
    <li>favicon</li> 

</ul>

```java
    webview.setWebChromeClient(new WebChromeClient(){
            /**
             * Notify the host application of a change in the document title.
             * @param view The WebView that initiated the callback.
             * @param title A String containing the new title of the document.这个是title
             */
            @Override
            public void onReceivedTitle(WebView view, String title) {
                super.onReceivedTitle(view, title);
            }
            /**
             * Notify the host application of a new favicon for the current page.
             * @param view The WebView that initiated the callback. 
             * @param icon A Bitmap containing the favicon for the current page. 这个就是标签图片
             */
            @Override
            public void onReceivedIcon(WebView view, Bitmap icon) {
                super.onReceivedIcon(view, icon);
            }
        });
```

<ul>
    <li>video</li> 
    <li>image</li>
</ul>
