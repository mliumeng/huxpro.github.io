---
layout:     post
title:      "DownloadManager 下载完成并安装"
subtitle:   "DownloadManager 是Android系统提供的一个很好用的下载类。通过此类可以很方便的下载文件并在通知栏显示进度，不用再重写通知栏。所以记录一下使用方法与一些技巧。望批判"
date:       2017-05-29 09:40:00
author:     "刘蒙"
header-mask: 0.3
catalog:    true
tags:
    - android
    - videobutton
    - button
    - Material
---
> 本篇迁移自我的CSDN 

## DownloadManager 下载完成并安装
---
> **话说blog还是要坚持写的。仅仅是一个态度的问题 .......**
 >>DownloadManager 是Android系统提供的一个很好用的下载类。通过此类可以很方便的下载文件并在通知栏显示进度，不用再重写通知栏。所以记录一下使用方法与一些技巧。望批判。将简单的下载执行的更简单！

### 直接上代码吧

```java
public void download(String downloadUrl) {
		DownloadManager manager = (DownloadManager) mContext
				.getSystemService(Context.DOWNLOAD_SERVICE);
		DownloadManager.Request request = new DownloadManager.Request(
				Uri.parse(downloadUrl));
		request.setDescription("更新APP");
		request.allowScanningByMediaScanner();// 设置可以被扫描到
		request.setVisibleInDownloadsUi(true);// 设置下载可见
		request.setNotificationVisibility(
                DownloadManager.Request.VISIBILITY_VISIBLE_NOTIFY_COMPLETED);//下载完成后通知栏任然可见
		String fileName = downloadUrl.substring(downloadUrl.lastIndexOf("/"));// 解析fileName
		request.setDestinationInExternalPublicDir(
				Environment.DIRECTORY_DOWNLOADS, fileName);// 设置下载位置，sdcard/Download/fileName
		long refernece = manager.enqueue(request);// 加入下载并取得下载ID  
		SharedPreferences sPreferences = mContext.getSharedPreferences(
				"downloadplato", 0);
		sPreferences.edit().putLong("plato", refernece).commit();//保存此次下载ID
		
	}
```
> 获取并保存此次下载ID为了方便监听下载完成，并处理相关下载后的事情，比如下载一个app，为了提高用户体验就要自动弹出安装界面。这时候我们就要进行下载监听

`DownLoadBroadcastReceiver.java`

```java
package com.android.browser;

import android.annotation.SuppressLint;
import android.app.DownloadManager;
import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;
import android.content.SharedPreferences;
import android.net.Uri;

import com.android.browser.core.LogUtils;
/**
 * 下载完成广播监听：比如下载APP
 * 
 * */
public class DownLoadBroadcastReceiver extends BroadcastReceiver {

	@SuppressLint("NewApi")
	public void onReceive(Context context, Intent intent) {
		long myDwonloadID = intent.getLongExtra(
				DownloadManager.EXTRA_DOWNLOAD_ID, -1);
		LogUtils.i("下载完成 ID = " + myDwonloadID);
		SharedPreferences sPreferences = context.getSharedPreferences(
				"downloadplato", 0);
		long refernece = sPreferences.getLong("plato", 0);
		if (refernece == myDwonloadID) {
			String serviceString = Context.DOWNLOAD_SERVICE;
			DownloadManager dManager = (DownloadManager) context
					.getSystemService(serviceString);
			Intent install = new Intent(Intent.ACTION_VIEW);
			Uri downloadFileUri = dManager
					.getUriForDownloadedFile(myDwonloadID);
			install.setDataAndType(downloadFileUri,
					"application/vnd.android.package-archive");
			install.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
			context.startActivity(install);
		}

	}

}

 // 记得注册广播
```


> 完成 Thank you 