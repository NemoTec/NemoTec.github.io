---
layout: post
title: 【PMS】(一)PackageItemInfo类
category: 技术
tags: Android源码
keywords: PackageItemInfo, Android源码, PMS, android.content.pm.PackageItemInfo
description: Android源码 android.content.pm.PackageItemInfo类的解析，PackageItemInfo代表一个应用包内所有组件项和通用信息的基类。该类提供最基本的属性集。
---

注：本文作者Nemo, http://nemotec.github.io  
&nbsp;  
&nbsp;  

### 一、包名
包名：``android.content.pm.PackageItemInfo``  
子类：(均在android.content.pm包中) ``ApplicationInfo``, ``InstrumentationInfo``, ``PermissionGroupInfo``, ``PermissionInfo``, ``ComponentInfo``.  
&nbsp;  

### 二、概述        
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Base class containing information common to all package items held by the package manager.  This provides a very common basic set of attributes: a label, icon, and meta-data.  This class is not intended to be used by itself; it is simply here to share common definitions between all items returned by the package manager.  As such, it does not itself implement Parcelable, but does provide convenience methods to assist in the implementation of Parcelable in subclasses.  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;PackageItemInfo, 代表一个应用包内所有组件项和通用信息的基类。该类提供最基本的属性集，如: label, icon, meta-data等。一般不会直接使用该类，它设计之初就是为了包内其它基本组件项提供统一的基本定义。它没有实现接口Parcelable, 但它提供了传Parcel型的构造函数，以及writeToParcel()方法给它的子类来实现PackageItemInfo内部这部分的成员的Parcel化。  
&nbsp;  

### 三、主要成员  
``public String name;``  
该组件项的公共名称，从"android:name"属性得到。  

``public String packageName;``  
该组件项所在的包的包名。

``public int labelRes;``  
指定该组件项的标签，string型的资源id，从"android:label"属性得到，如不设置则为0.  

``public CharSequence nonLocalizedLabel;``  

``public int icon;``  
指定该组件项的图标，drawable型的资源id，从"android:icon"属性得到，如不设置则为0.  

``public int banner;``  
指定该组件项的横幅，drawable型的资源id，从"android:banner"属性得到，如不设置则为0.  

``public int logo;``  
指定该组件项的logo，drawable型的资源id，比应用图标要大，ActionBar可以设置是否显示logo, 从"android:banner"属性得到，如不设置则为0.  

``public Bundle metaData;``  
对应AndroidManifest中的<meta-data>标签。只有<activity>, <activity-alias>, <service>, <receiver>, <application>标签中可能包含<meta-data>子标签。  

``public int showUserIcon;``  
默认值为UserHandle.USER_NULL，还可以是UserHandle.USER_OWNER。只有实例的引用来访问。  
&nbsp;  

### 四、主要方法  
``public PackageItemInfo()``  
``public PackageItemInfo(PackageItemInfo orig)``  
``protected PackageItemInfo(Parcel source)``  
构造方法.   

``public CharSequence loadLabel(PackageManager pm)``  
返回该组件项的标签，优先级依次为：nonLocalizedLabel, labelRes, name, packageName.  

``public Drawable loadIcon(PackageManager pm)``  
返回该组件项的图标，通过ApplicationPackageManager的loadItemIcon()方法获取icon对应的Drawable.  

``public Drawable loadBanner(PackageManager pm)``  
返回该组件项的横幅，通过ApplicationPackageManager的getDrawable()方法获取banner对应的Drawable, 如果banner为0, 返回loadDefaultBanner()的结果.  

``public Drawable loadLogo(PackageManager pm)``  
返回该组件项的大图标，通过ApplicationPackageManager的getDrawable()方法获取logo对应的Drawable, 如果logo为0, 返回loadDefaultLogo()的结果.  

``public Drawable loadDefaultIcon(PackageManager pm)``  
返回该组件的默认图标，通过ApplicationPackageManager的getDefaultActivityIcon()方法, 返回的结果是com.android.internal.R.drawable.sym_def_app_icon对应的Drawable.  

``protected Drawable loadDefaultBanner(PackageManager pm)``  
返回null.  

``protected Drawable loadDefaultLogo(PackageManager pm)``  
返回null.  

``public XmlResourceParser loadXmlMetaData(PackageManager pm, String name)``  
找到metaData中对应为name的资源id, 通过ApplicationPackageManager的getXml()方法, 返回该id对应的XmlResourceParser.  

``protected ApplicationInfo getApplicationInfo()``  
返回null.  

``public void writeToParcel(Parcel dest, int parcelableFlags)``  
为PackageItemInfo的子类Parcel化提供基类部分的成员的写入。  
&nbsp;  

### 五、调用情况
#### 1. PackageParser.parsePackageItemInfo()  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;parsePackageItemInfo()是PackageParser的私有方法，也是唯一的参数有PackageItemInfo的方法，它在PackageParser中三个地方被调用，分别用于解析标签<permission-group>生成PermissionGroupInfo, 解析<permission>生成PermissionInfo, 解析<permission-tree>生成PermissionInfo.  
&nbsp;  

#### 2. PackageParser.parseBaseApplication()  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;parseBaseApplication()是PackageParser的私有方法，用于解析标签<application>生成ApplicationInfo, 只在PackageParser中被私有方法parseBaseApk(),  我们知PMS中scanPackageLI()最后会走到这里，也就是开机扫描所有安装apk时会解析manifest。  
&nbsp;  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;可以看到PackageItemInfo类本身并没有被直接使用，更多的是它的子类、间接子类，不过它定义了组件的共同特征，为研究其它组件信息提供基础，组件信息均是public的，外部直接用该类实例来引用赋值。它的主要子类是ComponentInfo, ApplicationInfo在后面会继续研究，其它三个子类会大致过一遍。  
