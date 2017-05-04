---
layout:     post
title:      "WebView整理"
subtitle:   "The Next Generation Application Model For The Web - Progressive Web App"
date:       2017-05-04 17:00:00
author:     "Hux"
header-img: "img/post-bg-nextgen-web-pwa.jpg"
header-mask: 0.3
catalog:    true
tags:
    - android
    - webview
    - PWA
---

# WebView 篇
> webView 使用总结。常用的控件却总是记不住使用方法。每次上网找代码。因为使用的简单所以总是容易忽略，在此记录
## 一、 webview 基础用法：
> 我认为以下三点最为常用，可应付一般场景

<b>
<ul>
    <li> 加载网址</li>
    <li> 加载本地html</li>
    <li> 不跳转系统浏览器</li>
</ol>
</b>
### 加载网址
```java
String url = "http://www.baidu.com";
WebView webview = (WebView)findViewById(R.id.web);
webview.load(url);
```
### 加载本地HTML
```java
String data = "<html>
                    <body>
                      <h1>这是标题一</h1>
                    </body>
                </html>";
WebView webview = (WebView)findViewById(R.id.web);
webview.loadData(data,"text/html","utf-8");
```
### 不跳转到系统浏览器
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



