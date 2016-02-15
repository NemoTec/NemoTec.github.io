---
layout: post
title: 【Android源码-PMS】(二)ComponentInfo类
category: 技术
tags: Android源码
keywords: ComponentInfo, Android源码, PMS, android.content.pm.ComponentInfo
description: Android源码 android.content.pm.ComponentInfo类的解析，ComponentInfo, 代表一个应用内组件(如ActivityInfo, ServiceInfo, ProviderInfo)通用信息的基类。它设计是为了不同应用的组件共享统一的定义。
---

注：转载请注明来自Nemo, http://nemotec.github.io  
&nbsp;  
&nbsp;  

### 一、包名  
包名：``android.content.pm.ComponentInfo``  
父类：``android.content.pm.PackageItemInfo``  
子类：(均在android.content.pm包中) ``ActivityInfo``, ``ServiceInfo``, ``ProviderInfo``.  
&nbsp;  

### 二、概述  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Base class containing information common to all application components(ActivityInfo, ServiceInfo). This class is not intended to be used by itself; it is simply here to share common definitions between all application components.  As such, it does not itself implement Parcelable, but does provide convenience methods to assist in the implementation of Parcelable in subclasses.  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ComponentInfo, 代表一个应用内组件(如ActivityInfo, ServiceInfo, ProviderInfo)通用信息的基类。一般不会直接使用该类，它设计是为了不同应用的组件共享统一的定义。它没有实现接口Parcelable, 但它提供了传Parcel型的构造函数，以及writeToParcel()方法给它的子类来实现ComponentInfo内部这部分的成员的Parcel化。  
&nbsp;  

### 三、主要成员  
``public ApplicationInfo applicationInfo;``  
组件所在的application/package信息, 从<application>标签得到。

``public String processName;``  
组件所运行的进程名，string型，从"android:process"属性得到，如不设置则为applicationInfo.processName.

``public int descriptionRes;``  
组件的描述，string型的资源id，从"android:description"属性得到，如不设置则为0.

``public boolean enabled = true;``  
表明当前组件能否被实例化，boolean型，从"android:enabled"属性得到。如果它所在的ApplicationInfo中enabled为false, 则这处的设置无效。

``public boolean exported = false;``  
表明当前组件能否被其它Application的组件启动，boolean型，从"android:exported"属性得到。 

如果当前组件没有一个<intent-filter>则它默认为false(没有任何<intent-filter>表明要用组件的准确名称来启动), exported=false表明当前组件只能被当前应用内组件启动，或有相同UID的应用。  

当然也可以用permission来限制外部应用对该组件的访问。如果该组件有"android:permission"属性，则访问者必须有声明该权限。当该组件无permission属性而<application>标签有声明时，则访问者必须有<application>标签声明的permission.  
&nbsp;  

### 四、主要方法  
``public ComponentInfo()``  
``public ComponentInfo(ComponentInfo orig)``  
``protected ComponentInfo(Parcel source)``  
构造函数.  

``public CharSequence loadLabel(PackageManager pm)``  
返回该组件的标签，CharSequence类型，优先级依次：nonLocalizedLabel, labelRes, applicationInfo.nonLocalizedLabel, applicationInfo.labelRes.  

``public boolean isEnabled()``  
当且仅当enabled和applicationInfo.enabled同时为true, 返回true.

``public final int getIconResource()``  
``public final int getLogoResource()``  
``public final int getBannerResource()``  
返回该组件的icon, logo, banner, int型的资源id, 如果某项为0, 则返回applicationInfo对应的资源id.

``public Drawable loadDefaultIcon(PackageManager pm)``  
``protected Drawable loadDefaultBanner(PackageManager pm)``  
``protected Drawable loadDefaultLogo(PackageManager pm)``  
返回该组件的默认icon, logo, banner, int型的资源id, 分别返回的是applicationInfo的loadIcon(), loadBanner(), loadLogo().

``protected ApplicationInfo getApplicationInfo()``  
返回该组件的applicationInfo.  

``public void writeToParcel(Parcel dest, int parcelableFlags)``  
先调用父类PackageItemInfo的writeToParcel()方法，再完成自己的5个成员的Parcel化。  
&nbsp;  

### 五、调用情况
#### 1. PackageParser.Component  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;PackageParser中的组件基类Component，它的一个构造函数会传入ComponentInfo的子类，内部对它的成员进行赋值。PackageParser有六个组件继承自Component基类。  
&nbsp;  

#### 2. ResolveInfo  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ResolveInfo更像一个工厂类，它内部有成员ActivityInfo activityInfo, ServiceInfo serviceInfo, ProviderInfo providerInfo; 而这三个类均继承自ComponentInfo，ResolveInfo也是通过ComponentInfo型引用来动态操作这三个组件信息类。  
&nbsp;  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ComponentInfo也没有直接使用的情况，它更多是作为一个基类动态地被赋予它三个子类的实例，来达到多态的效果。  
