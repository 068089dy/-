---
layout: post
title:  "android导入moudle踩坑"
category: "note"
keywords: ["android"]
description: "android导入moudle踩坑"
tags: ["android"]
---
## 前言
之前只导入过现成的moudle，没有把一个现成的工程当作moudle导入过，今天踩了一下坑。
## 需要注意的细节：
### 1.gradle的修改
将moudle工程gradle文件头部的
```
apply plugin: 'com.android.application'
```
修改为
```
apply plugin: 'com.android.library'
```
修改app的gradle文件，在dependencies中添加：
```
//daniulib是moudle名
compile project(':daniulib')
```
### 2.manifest文件的修改
权限部分无须修改，application部分不能有activity，不然app启动可能会打开moudle中的activity，application修改为这样：
```
<application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:supportsRtl="true">
</application>
```
