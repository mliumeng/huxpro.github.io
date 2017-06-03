---
layout:     post
title:      "Facebook,Twitter,Google+ 第三方登陆最详细教程"
subtitle:   "滴水穿石。厚积薄发。技术在于积累，在于梳理。更在于分享。如果你是面对海外的市场，总是要使用Facebook，Twitter，Google+ 这三大社交平台来实现第三方登陆，本文章将详细接收其使用方法。"
date:       2017-06-03 9:22:00
author:     "刘蒙"
header-img: "img/facebook_header.jpg"
header-mask: 0.3
catalog:    true
tags:
    - android
    - facebook
    - twitter
    - googlt
    - 第三方登陆
---

# Facebook,Twitter,Google+ 第三方登陆最详细教程
滴水穿石。厚积薄发。技术在于积累，在于梳理。更在于分享。如果你是面对海外的市场，总是要使用Facebook，Twitter，Google+ 这三大社交平台来实现第三方登陆，本文章将详细接收其使用方法。
欢迎转载：转载请注明出处
http://www.jianshu.com/p/7e69d8dd4e07
## 先来一个官方直通车（自备梯子）：
-  [Facebook LoginButton 官方集成文档](https://developers.facebook.com/docs/facebook-login/android)
-  [Twitter LoginButton 官方集成步骤 ](https://fabric.io/kits/android/twitterkit/install)
-  [Google+ LoginButton 官方集成文档](https://developers.google.com/identity/sign-in/android/)
```// 如果你不想看外国人写的东西。往下看，有现成的解释。```
--------------------
## 一 、Facebook LoginButton
下面有一群图文讲解：
### 第一步：点击上面直通车[Facebook官方文档](https://developers.facebook.com/docs/facebook-login/android) ![第一步](http://upload-images.jianshu.io/upload_images/2820692-e544e79f8724b92f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 第二步：注册为开发者 ![第二步](http://upload-images.jianshu.io/upload_images/2820692-709a60d2cb233c01?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 第三步：添加你的应用 ![第三步](http://upload-images.jianshu.io/upload_images/2820692-544f40239273f949?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 第四步：创建应用
 > 注意这里app名称不能含有facebook fb 等，详见第五步
>![第四步](http://upload-images.jianshu.io/upload_images/2820692-b852786b73619c12?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 第五步：创建应用编号
> 注意：要选择应用分类！！ 
>![第五步](http://upload-images.jianshu.io/upload_images/2820692-0dbc0d07866e7f10?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 

### 第六步：完善应用信息
> 这里注意
> -  按照import SDK 完成应用添加后再近行addSDK> - Import SDK 较为简单，在此就省略截图
>![第六步](http://upload-images.jianshu.io/upload_images/2820692-b582caceaa3102a2?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 第七步：Add key hashes
> 如果说简单，这步也很简单，就是打开终端输入一下面命令就行
> -  难就难在命令不好输：
> - keytools 大家都有，然而openssl 不知道你电脑是否安装，如果有就能直接输命令了 
- 如果没有，先安装openssl再说，安装openssl之前，确保自己的visual studio安装正确，环境正常 -  安装openssl[参考链接](http://blog.csdn.net/houjixin/article/details/25806151) -  ![重点](http://upload-images.jianshu.io/upload_images/2820692-86c467c6be71d536?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### facebook 最后一步：
> 参考文档，将代码复制到自己的项目中即可，结尾会留demo供大家参考！

## 二、Twitter LoginButton

### 第一步：注册开发者账户
[注册开发者账户传从门](https://apps.twitter.com/)登陆你的Twitter并完成注册
### 第二步：创建App
> - create new app(三步)
>> ![create new app](http://upload-images.jianshu.io/upload_images/2820692-f197a748fb1a745c?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
> - 完善app详细信息 > ![create new app](http://upload-images.jianshu.io/upload_images/2820692-237771887219ea5a?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
> - 完成创建> ![create new app](http://upload-images.jianshu.io/upload_images/2820692-ca735d3755798e68?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 第三步：添加代码到你的项目中
> 点击上面的直达车，到[Twitter官方集成步骤](https://fabric.io/kits/android/twitterkit/install)
> 你也可以[下载官方插件](https://fabric.io/downloads/android-studio)，自动完成这一步骤（android studio）
> 结尾会留demo供大家参考！

## 三、Google+ LoginButton
### 第一步：直通车[Google+官方集成文档](https://developers.google.com/identity/sign-in/android/start)
> Google+ 集成较为简单，因为有官方demo可以参考
> >-  下载demo 通过git check out the samples>
`$ git clone https://github.com/googlesamples/google-services.git`
>- Open Android Studio.>Select File > Open, browse to where you cloned the google-services repository, and open google-services/android/signin

### 第二步：创建应用- 直通车[Google+官方集成文档](https://developers.google.com/identity/sign-in/android/start)![start](http://upload-images.jianshu.io/upload_images/2820692-6b762a8dfadc3216?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 创建或选择app![create app](http://upload-images.jianshu.io/upload_images/2820692-0c3beb425ca1bf33?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 选择服务![create app](http://upload-images.jianshu.io/upload_images/2820692-ba3702ecb0f3adbf?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#### 这里讲怎么获取SHA-1
>  -  获取sha-1 命令
> ![sha-1](http://upload-images.jianshu.io/upload_images/2820692-f2ab6c157c233b82?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>- 下载配置文件
>![get configer file](http://upload-images.jianshu.io/upload_images/2820692-3d6a5753fec31b7f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>>此处，我犯了一个不知道大家会不会犯的错误。。。
>我以为命令是这样
>`keytool -exportcert -list -v \-alias androiddebugkey -keystore %USERPROFILE%\.android\debug.keystore`
>实际上是：如图↓
>![get key](http://upload-images.jianshu.io/upload_images/2820692-7d3aaadd14496c59?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 最后：好了，我不想写了，看demo
---
> 多谢大家！如有不足，请无情的指出，感谢！-- IT小学生
>欢迎转载：转载请注明出处http://www.jianshu.com/p/7e69d8dd4e07

#[点击查看demo](https://github.com/mliumeng/Singin)
end