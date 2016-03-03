---
layout: post
title: 【PMS】(三)ApplicationInfo类解析
category: 技术
tags: Android源码
keywords: ApplicationInfo, Android源码, PMS, android.content.pm.ApplicationInfo
description: Android源码 android.content.pm.ApplicationInfo类的解析。ApplicationInfo, 通过它可以得到一个应用基本信息。本文将解析所有ApplicationInfo的来源，传递，使用。
---

注：转载请注明来自Nemo, http://nemotec.github.io  
&nbsp;  
&nbsp;  

### 一、包名  
包名：``android.content.pm.ApplicationInfo``  
父类：``android.content.pm.PackageItemInfo``  
接口：``android.os.Parcelable``   
子类：无  
&nbsp;  

### 二、概述        
Information you can retrieve about a particular application. This corresponds to information collected from the AndroidManifest.xml's \<application\> tag.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**ApplicationInfo**, 通过它可以得到一个应用基本信息。这些信息是从AndroidManifest.xml的\<application>标签获取的。  
&nbsp;  

### 三、主要成员  
``public String taskAffinity;``  
String型，当前应用所有Activity的默认task密切性。可以参考ActivityInfo的taskAffinity,从"**android:taskAffinity**"属性得到。具体taskAffinity是怎么影响到Activity在task的启动, 后面会在Activity启动模式中细讲。  

``public String permission;``  
String型，可选项,访问当前应用所有组件需要声明的权限，从"**android:permission**"属性得到。  

``public String processName;``  
String型，从"**android:process**"属性得到，注明应用运行的进程名。或不设置则默认为应用包名。  

``public String className;``  
String型，从"**android:class**"属性得到，应用实现的Application类的类名。  

``public int descriptionRes;``  
int型，从"**android:description**"属性得到，对Application组件的描述。若不设置则为0。  

``public int theme;``  
int型，从"**android:theme**"属性得到，注明应用的主题。若不设置则为0。  

``public String manageSpaceActivityName;``  
String型，从"**android:manageSpaceActivity**"属性得到，用于指定一个Activity来管理数据，它最终会出现在设置->应用程序管理中，默认为按钮为"清除数据"，指定此属性后，该按钮可点击跳转到该Activity, 让用户选择性清除哪些数据。若不设置则为null。  

``public String backupAgentName;``  
String型，从"**android:backupAgent**"属性得到，Android原生的备份引擎BackupManagerService在应用端的实现类，是backupAgent的子类。默认不会由系统备份，若"**android:allowBackup**"值为false, 则该属性设置无效。  

``public int fullBackupContent = 0;``  
int型，可选项，注明应用是否支持自动备份。  

``public int uiOptions = 0;``  
int型，为应用内所有Activity设置的默认UI选项，可选值为``"none"``, ``"splitActionBarWhenNarrow"``。  

``public int flags = 0;``  
int型，应用manifest中设置的各项属性的按位或的组合, 共31个：  

| manifest | FLAG   | value  |
| :-----   | :----- | :----- |
| 123      | 123    | 123    |

xxxxxxxx  

|manifest|FLAG|value|
|-----|-----|-----|
|123|123|123|  

yyyyyyyy  



First Header | Second Header
------------ | -------------
Content from cell 1 | Content from cell 2
Content in the first column | Content in the second column




android:debuggable | FLAG_DEBUGGABLE | 1<<1  

11111111



==system/app== | FLAG_SYSTEM | 1<<0  
android:debuggable | FLAG_DEBUGGABLE | 1<<1  

android:hasCode | FLAG_HAS_CODE | 1<<2  

android:persistent|FLAG_PERSISTENT|1<< 3
==factory test mode==|FLAG_FACTORY_TEST|1<<4
android:allowTaskReparenting|FLAG_ALLOW_TASK_REPARENTING|1<<5
android:allowClearUserData|FLAG_ALLOW_CLEAR_USER_DATA| 1<<6
==an update to a built-in==|FLAG_UPDATED_SYSTEM_APP|1<<7
android:testOnly|FLAG_TEST_ONLY|1<<8
android: smallScreens|FLAG_SUPPORTS_SMALL_SCREENS|1<<9
android:normalScreens|FLAG_SUPPORTS_NORMAL_SCREENS|1<<10
android:largeScreens|FLAG_SUPPORTS_LARGE_SCREENS|1<<11
android:resizeable|FLAG_RESIZEABLE_FOR_SCREENS|1<<12
android:anyDensity|FLAG_SUPPORTS_SCREEN_DENSITIES|1<<13
android:vmSafeMode|FLAG_VM_SAFE_MODE|1<<14
android:allowBackup|FLAG_ALLOW_BACKUP|1<<15
android:killAfterRestore|FLAG_KILL_AFTER_RESTORE|1<<16
android:restoreAnyVersion|FLAG_RESTORE_ANY_VERSION|1<<17
==installed on removable storage==|FLAG_EXTERNAL_STORAGE|1<<18
android:xlargeScreens|FLAG_SUPPORTS_XLARGE_SCREENS|1<<19
android:largeHeap|FLAG_LARGE_HEAP|1<<20
==package in stopped state==|FLAG_STOPPED|1<<21
==support RTL(right to left)==|FLAG_SUPPORTS_RTL|1<<22
==is currently installed==|FLAG_INSTALLED|1<<23
==only data installed==|FLAG_IS_DATA_ONLY|1<<24
==declared to be a game==|FLAG_IS_GAME|1<<25
==full-data streaming backups==|FLAG_FULL_BACKUP_ONLY|1<<26
android:usesCleartextTraffic|FLAG_USES_CLEARTEXT_TRAFFIC|1<<27
==set installer extracts native libs from .apk files==|FLAG_EXTRACT_NATIVE_LIBS|1<<28
==hardware accelerated==             |FLAG_HARDWARE_ACCELERATED|1<<29
==support multiple instruction sets==|FLAG_MULTIARCH|1<< 31  

``public String sourceDir;``  
String型，应用APK的全路径。  

``public String publicSourceDir;``  
String型，sourceDir公开可访问的部分。被forward lock锁定的应用该值可能与sourceDiri不一样。  

``public String[] resourceDirs;``  
String型，当前应用有额外资源包时，该资源的全路径。通常为null。  

``public String seinfo;``  
String型，来自SELinux策略中``seinfo``标签，这个值一般在设置应用进程的SELinux安全上下文时有用。  

``public String dataDir;``  
String型，应用数据目录。  

``public String nativeLibraryDir;``  
String型，JNI本地库存放路径。  

``public int uid;``  
int型，``kernel user-ID``，目前对每个应用还不是唯一的，存在几个应用共享一个UID的情况。  

``public int targetSdkVersion;``  
int型，最低的目标SDK版本号。  

``public int versionCode;``  
int型，应用的版本号。  

``public boolean enabled = true;``  
boolean型，标明当前应用所有组件是否可用。  
&nbsp;  

### 四、主要方法  
``public ApplicationInfo()``  
``public ApplicationInfo(ApplicationInfo orig)``  
``private ApplicationInfo(Parcel source)``  
构造函数，同父类PackageItemInfo一样ApplicationInfo也提供三个构造函数，一个无参的，一个参数为另一个ApplicationInfo，一个参数为Parcel。  

``public void writeToParcel(Parcel dest, int parcelableFlags)``  
实现Parcelable接口。  

``protected ApplicationInfo getApplicationInfo()``  
通过``return this``返回当前ApplicationInfo实例。  

``public boolean isSystemApp()``  
判断当前应用是否为系统应用。``flags``与``ApplicationInfo.FLAG_SYSTEM``按位与不等于0则返回true.  

``public boolean isForwardLocked()``  
判断当前应用是否被锁定。``privateFlags``与``ApplicationInfo.PRIVATE_FLAG_FORWARD_LOCK``按位与不等于0返回true.  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;还有一些其它方基本也是返回当前ApplicationInfo实例的成员，以及一些isxxx方法，它们是flags与某项预定义FLAG按位与后返回的结果。  
&nbsp;  

### 五、调用情况
#### 1. 解析manifest得到ApplicationInfo信息  
(1) **PackageParser中parseBaseApk()方法，会解析指定路径apk的AndroidManifest.xml文件**，遇到tag为"application"时，会调用``parseBaseApplication()``方法来完成接下来的节点解析工作。可以看到最后的解析结果都会存放在``parseBaseApplication()``传入的参数owner中，它是``PackageParser.Package``类型。也就是**所有ApplicationInfo信息的来源均是从这个传出的Package得来。**  

```
【PackageParser】parseBaseApk(三参)  
 ↓  
【PackageParser】parseBaseApk(四参)  
 ↓  
【PackageParser】parseBaseApplication()  
```
&nbsp;  

(2) PackageManagerService中, ``scanPackageLI(File...)``它在每次重启后PMS会扫描已安装的apk, 这里会调用``PackageParser.parsePackage()``方法，这样就走到(1)中, 解析出ApplicationInfo信息存放在传出的Package中，接着会用返回的Package实例为参数调用到``scanPackageLI(PackageParser.Package...)``, 最终会在``scanPackageDirtyLI()``中通过``mPackages.put(pkg.applicationInfo.packageName, pkg)``, 把新解析的Package以packageName为Key,  存放在HashMap实例``mPackages``中。 
 
```
【PackageManagerService】scanPackageLI(File...)  
 ↓  
【PackageParser】parsePackage()  
 ↓  
【PackageParser】parseBaseApk()  
 ....  
【PackageManagerService】scanPackageLI(PackageParser.Package...)  
 ↓  
【PackageManagerService】scanPackageDirtyLI(): mPackages.put(pkg.applicationInfo.packageName, pkg);
```
&nbsp;  

(3) 这里``installPackageLI()``是外部调用PackageManager的方法``installPackage()``时，走到PackageManagerService内部会走到的方法，可以看到最后还是走到(2)中讲到的``scanPackageLI()``，这样新安装的apk对应的Package也会被添加到PackageManagerService的``mPackages``中去。  

```
【PackageManagerService】installPackageLI()  
 ↓  
【PackageManagerService】installNewPackageLI()  
 ↓  
【PackageManagerService】scanPackageLI(PackageParser.Package...)  
```
&nbsp;  

**总结1：任一时刻，系统中安装过的apk都会被解析完成，包信息被存放在PackageManagerService中的``mPackages``，它是以包名为Key, 以``PackageParser.Package``为Value的HashMap。当然每个新安装的应用的ApplicationInfo也是在从最初被解析出来，最后存放在这里的。**  
&nbsp;  

#### 2. PackageManagerService中传出ApplicationInfo的方法
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;从上一步分析可知，所有应用的ApplicationInfo均是存放在PackageManagerService的mPackages成员中，所以要取得某个应用的ApplicationInfo必须通过mPackages.get(packageName);先取到该packageName的Package. 这样就可以在PackageManagerService中查找有哪些地方这样调用了。  

```
(1) public ApplicationInfo getApplicationInfo(String packageName, int flags, int userId)
{    
    synchronized (mPackages) {
        PackageParser.Package p = mPackages.get(packageName);
        ......
        return PackageParser.generateApplicationInfo(p, flags, ps.readUserState(userId), userId);
        ......
        if ("android".equals(packageName) || "system".equals(packageName)) {
            return mAndroidApplication;
        }
    }
}
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;可以看到PackageManagerService中的``getApplicationInfo()``方法是取``mPackages``中Key为传入的包名对应的Value, 然后用``PackageParser.generateApplicationInfo()``方法来生成ApplicationInfo并返回。  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;``generateApplicationInfo()``方法中主要有一句: ``ApplicationInfo ai = new ApplicationInfo(p.applicationInfo)``; 传给外部的是一份拷贝，而不是直接传递给外部``p.applicationInfo``, 当然也有特殊情况。  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;外部调用``getApplicationInfo()``一般是通过``PackageManager.getApplicationInfo()``, 而它的实现最终是在ApplicationPackageManager中通过IPackageManager调用到``PackageManagerService.getApplicationInfo()``。  
&nbsp;  

**(2) public ParceledListSlice \<ApplicationInfo\> getInstalledApplications(int flags, int userId)**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;它是取出``mPackages``中所有的Package,然后通过``PackageParser.generateApplicationInfo()``传出每个Package的applicationInfo成员。  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;同理在PackageManager中也有同名方法，外部通过``PackageManager.getInstalledApplications()``方法来获取所有已安装的包的ApplicationInfo。  
&nbsp;  

**(3) public List \<ApplicationInfo\> getPersistentApplications(int flags)**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这里返回具有常驻属性的ApplicationInfo。同理也是从``mPackages``中取出``Package p``, 如果p的``applicationInfo.flags``有ApplicationInfo.FLAG_PERSISTENT属性，就把它加入到结果列表中，当然对于每一个符合条件的Package, 也是通过``PackageParser.generateApplicationInfo()``分别传出其applicationInfo成员。  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**这个方法只在ActivityManagerService的systemReady()方法中，通过接口``IPackageManager.getPersistentApplications()``调用过一次。**  
&nbsp;  

**(4) public PackageInfo getPackageInfo(String packageName, int flags, int userId)**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;外部调用``getPackageInfo()``同样是通过``PackageManager.getPackageInfo()``, 而它的实现最终是在ApplicationPackageManager中通过IPackageManager调用到``PackageManagerService.getPackageInfo()``。  

```
public PackageInfo getPackageInfo(String packageName, int flags, int userId) {
    ....
    synchronized (mPackages) {
        PackageParser.Package p = mPackages.get(packageName);
        ....
        if (p != null) {
            return generatePackageInfo(p, flags, userId);
        }
        ....
    }
    return null;
}
```  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;PackageManagerService中``getPackageInfo()``方法同样从``mPackages``中取packageName对应的``Package p``, 把它传入``generatePackageInfo()``。间接调用的是``PackageParser.generatePackageInfo()``, 它其中有一句：
``pi.applicationInfo = generateApplicationInfo(p, flags, state, userId);``所以最终也是取得``mPackages``中的一个Package的applicationInfo, 用它来构造传出的``PackageInfo.applicationInfo``。  
&nbsp;  

#### 3. 真正new出ApplicationInfo  
(1) PackageParser中最终new出新Application实例的地方就是通过``generateApplicationInfo()``方法：  

```
public static ApplicationInfo generateApplicationInfo(Package p, int flags, PackageUserState state, int userId)  
public static ApplicationInfo generateApplicationInfo(ApplicationInfo ai, int flags, PackageUserState state, int userId)
```
&nbsp;  

(2) PackageManagerService中以下方法就是调用PackageParser的``generateApplicationInfo()``方法或通过对应generateXXX()方法间接调用到``generateApplicationInfo()``.  

```
PackageManagerService:  
public ApplicationInfo getApplicationInfo(String packageName, int flags, int userId) 
public ParceledListSlice<ApplicationInfo> getInstalledApplications(int flags, int userId)  
public List<ApplicationInfo> getPersistentApplications(int flags)  
public PackageInfo getPackageInfo(String packageName, int flags, int userId)  
public ActivityInfo getActivityInfo(ComponentName component, int flags, int userId)
public ActivityInfo getReceiverInfo(ComponentName component, int flags, int userId)
public ServiceInfo getServiceInfo(ComponentName component, int flags, int userId)
public ProviderInfo getProviderInfo(ComponentName component, int flags, int userId)
```
&nbsp;  

(3) PackageManagerService中``getActivityInfo()``等方法调用到的PackageParser对应的``generateActivityInfo()``等。它们最终都是通过``generateApplicationInfo()``生成对外的ApplicationInfo实例。  

```
PackageParser:
public static PackageInfo generatePackageInfo(PackageParser.Package p, ...)
public static final ActivityInfo generateActivityInfo(Activity a, int flags, PackageUserState state, int userId)  
public static final ActivityInfo generateActivityInfo(ActivityInfo ai, int flags, PackageUserState state, int userId)  
public static final ServiceInfo generateServiceInfo(Service s, int flags, PackageUserState state, int userId)  
public static final ProviderInfo generateProviderInfo(Provider p, int flags, PackageUserState state, int userId)  
```
&nbsp;  

总结：外部拿到ApplicationInfo信息的过程如下：  

```
【PackageManager】getActivityInfo()  
 ↓  
【ApplicationPackageManager】getActivityInfo()  
 ↓  
【IPackageManager】getActivityInfo()  
 ↓  
【PackageManagerService】getActivityInfo()  
 ↓  
【PackageParser】generateActivityInfo()  
 ↓  
【PackageParser】generateApplicationInfo()  
```

其它的PackageInfo, ServiceInfo, ProviderInfo等过程相同。  
&nbsp;  

#### 4. 其它类的成员变量  
**(1) 【ActivityRecord】**[``final ApplicationInfo appInfo;``]
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ActivityRecord对appInfo成员进行赋值的地方只有其构造函数``appInfo = aInfo.applicationInfo;``我们搜索哪些地方会new ActivityRecord实例:  

a. ``ActivityStackSupervisor.startActivityLocked()``，这里是启动Activity过程中的一个方法，这里new ActivityRecord()的参数中传入的ActivityInfo实例是外部传入的，这个ActivityInfo是在``startActivityMayWait()``生成的，它最终是通过``PackageManagerService.resolveIntent()``得到ResolveInfo实例，返回``rInfo.activityInfo``.  

b. ``restoreFromXml()``，这个函数也只有一处调用，它是Android的重启恢复Task功能，在``ActivityManagerService.systemReady()``有一句``mRecentTasks.addAll(mTaskPersister.restoreTasksLocked());``TaskPersister的restoreTasksLocked()方法会读取存储的Task文件目录，递归解析每个文件，通过``TaskRecord.restoreFromXml(in, mStackSupervisor)``分别生成TaskRecord, 其中一句``ActivityRecord activity = ActivityRecord.restoreFromXml(in, stackSupervisor);``在restoreFromXml()中
``ActivityInfo aInfo = stackSupervisor.resolveActivity(intent, resolvedType, 0, null, userId);``最后把aInfo作为参数构造ActivityRecord实例。  
&nbsp;  

**(2) 【ServiceRecord】**[``final ApplicationInfo appInfo;``]  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ServiceRecord对其ApplicationInfo成员appInfo赋值同样只在其构造函数: ``appInfo = sInfo.applicationInfo;``new ServiceRecord的地方只有一处``ActiveServices.retrieveServiceLocked()``，再看构造函数传入的sInfo是从哪来的，``ResolveInfo rInfo = AppGlobals.getPackageManager().resolveService()``, 然后传入的sInfo是``rInfo.serviceInfo``.  
&nbsp;  

**(3) 【ContentProviderRecord】**[``final ApplicationInfo appInfo;``]  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;只在ActivityManagerService中有new ContentProviderRecord实例，``getContentProviderImpl()``和``generateApplicationProvidersLocked()``分别为生成某个包指定名称的Provider信息，和生成某个应用进程所有Provider信息，构造函数传入参数均为某ProcessRecord的info成员。  
&nbsp;  

**(4) 【ProcessRecord】**[``final ApplicationInfo info;``]  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;初始化这个成员info的地方只有在ProcessRecord的构造函数，全局搜索``new ProcessRecord()``的地方，只在``ActivityManagerService.newProcessRecordLocked()``.
调用它的地方只在ActivityManagerService:  

a. setSystemProcess()&nbsp;&nbsp;它会通过``PackageManager.getApplicationInfo()``, 最终是调用``PackageManagerService中.getApplicationInfo()``, 得到包名为"android"的Application对象，这里是在创建"system_server"进程时构建系统的ProcessRecord中成员info的过程。   

b. startProcessLocked(14个参数版本)&nbsp;&nbsp;这个函数是启动一个新进程过程中的一环，在查找没有是否有已创建的packageName对应的ProcessRecord对象，如果没有就创建一个。新建的ProcessRecord也会在``newProcessRecordLocked()``经由``addProcessNameLocked()``添加到``ProcessMap< ProcessRecord> mProcessNames``成员中。新建ProcessRecord的参数也是由info传递，为``ActivityRecord.info.applicationInfo``。  

c. addAppLocked()&nbsp;&nbsp;主要是处理具有persist属性的应用进程，它传入的参数是``ProcessRecord.info``.  
&nbsp;  

**(5) 【BackupRecord】**[``final ApplicationInfo appInfo;``]  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;只在``ActivityManagerService.bindBackupAgent()``方法中会去new BackupRecord, 其构造函数传入ApplicationInfo参数是由bindBackupAgent()传入。  
&nbsp;  

**(6) 【ComponentInfo】**[``public ApplicationInfo applicationInfo;``]  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;一般不会直接构造ComponentInfo，外部是使用其子类ActivityInfo, ServiceInfo, ProviderInfo, 由前面可知，它们的成员变量applicationInfo最后均是调用到PackageParser的generateXXX()方法构造。  
&nbsp;  

**(7) 【PackageInfo】**[``public ApplicationInfo applicationInfo;``]  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;PackageInfo的成员applicationInfo赋值过程与ActivityInfo类似，最终是PackageParser的generatePackageInfo()方法构造。  
&nbsp;  

**(8) 【ActivityThread.AppBindData】**[``ApplicationInfo appInfo;``]  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;只在ActivityThread中使用，ActivityThread有一个AppBindData类型的成员mBoundApplication。在``bindApplication()``方法中，会new一个AppBindData，它的成员appInfo被传入的ApplicationInfo参数初始化。最终这个AppBindData实例会在``handleBindApplication(AppBindData data)``中``mBoundApplication = data;``对mBoundApplication初始化完成。也就是在ActivityThread中成员mBoundApplication会存放本应用的ApplicationInfo信息。  
&nbsp;  

**(9) 【LoadedApk】**[``private ApplicationInfo mApplicationInfo;``]  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;我们知道一个LoadedApk对应一个加载的apk, 它的成员mApplicationInfo就是该应用的应用信息，对mApplicationInfo赋值分以下两种情况：  

**a. 创建系统"android"包的ApplicationInfo**:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;new一个SystemServer的LoadedApk，间接创建一个了包名为"android"的ApplicationInfo。  

```
【SystemServer】main()  
 ↓  
【SystemServer】run()  
 ↓  
【SystemServer】createSystemContext()  
 ↓  
【ActivityThread】getSystemContext()  
 ↓  
【ContextImpl】createSystemContext()  
 ↓  
【LoadedApk】LoadedApk(mainThread)  
```  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;接着在``SystemServer.run()``中，调用到方法``startBootstrapServices()``，在AMS的``setSystemProcess()``中，``ApplicationInfo info = mContext.getPackageManager().getApplicationInfo("android", STOCK_PM_FLAGS);``一句会拿到最后写入LoadedApk的ApplicationInfo实例。它实际是走到PMS的``getApplicationInfo("android", ...)``，其中对于"android"包的处理：  

```
if ("android".equals(packageName)||"system".equals(packageName)) {
    return mAndroidApplication;
}
```  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;后面PMS对mAndroidApplication赋值也只有一处，在``scanPackageDirtyLI()``中，是扫描系统中安装的apk包名为"android"，后面我们会知道它其实就是"framework-res.apk"。  

```
【SystemServer】startBootstrapServices()  
 ↓  
【ActivityManagerService】setSystemProcess()  
 ↓  
【ActivityThread】installSystemApplicationInfo()  
 ↓  
【ContextImpl】installSystemApplicationInfo()  
 ↓  
【LoadedApk】installSystemApplicationInfo()  
```  

**b. 创建单个应用的ApplicationInfo**:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;new一个应用的LoadedApk会在``ActivityThread.getPackageInfoNoCheck()``方法中, 构造它传入的ApplicationInfo参数一般就是启动应用某个组件时，该组件信息，比如ActivityInfo, ServiceInfo等。  
&nbsp;  

**(10) 【PackageParser.Package】**[``public final ApplicationInfo applicationInfo;``]  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;前面分析知``scanPackageLI()``和``installPackageLI()``会去构造``PackageParser.Package``，而最终是在``parseBaseApplication()``中解析Manifest文件，它相关应用信息存放到Package的成员applicationInfo。  
&nbsp;  

**(11) 【PackageManagerService】**[``ApplicationInfo mAndroidApplication;``]  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对PackageManagerService的成员mAndroidApplication的赋值只在``scanPackageDirtyLI()``方法中：  

```
if (pkg.packageName.equals("android")) {
    ....
    synchronized (mPackages) {
        ....
        mAndroidApplication = pkg.applicationInfo;
    }
}
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;而这个包名为"android"的apk其实是framework-res.apk,也就是PackageManagerService中mAndroidApplication存放的是framework-res.apk的ApplicationInfo信息。  
&nbsp;  
&nbsp;  

**总结：**  
**1. 任一时刻，系统中安装过的apk都会被解析完成，包信息被存放在PackageManagerService中的``mPackages``，它是以包名为Key，以``PackageParser.Package``为Value的HashMap。PMS中``scanPackageLI()``开机扫描已安装的apk，installPackageLI()安装新apk，最终都会走到``PackageParser.parseBaseApplication()``解析应用manifest文件，在scanPackageDirtyLI()中添加到``mPackages``.其它地方获取ApplicationInfo实例，都会间接地调用到``PackageParser.generateApplicationInfo()``.**  
&nbsp;  

**2. 系统中包名为"android"的apk其实是framework-res.apk(也可查看frameworks\base\core\res\AndroidManifest.xml中定义的包名),同时PackageManagerService中mAndroidApplication存放的是framework-res.apk的ApplicationInfo信息。framework-res.apk是"system_server"进程加载的apk.**  
&nbsp;  

**3. framework-res.apk加载过程：``SystemServer.createSystemContext()`` -> ``ActivityThread.getSystemContext()`` -> ``ContextImpl.createSystemContext()`` -> ``new LoadedApk()`` -> ``Resources.getSystem()`` -> ``AssetManager.getSystem()`` -> ``new AssetManager(true)`` -> ``android_util_AssetManager.android_content_AssetManager_init()`` -> ``android_util_AssetManager.verifySystemIdmaps()``.**  
&nbsp;  

**4. ActivityManagerService中，所有已运行的应用，它的应用记录都会被存放在``ProcessMap<ProcessRecord> mProcessNames``成员中，它是以包名为key，以ProcessRecord为value的Map，要需要启动某个新的应用进程时，会先查找mProcessNames是否已有其记录，没有才去``new ProcessRecord()``。**  
&nbsp;  

**5. 大多情况下得到一个ApplicationInfo实例是通过以下：``ActivityRecord.info.applicationInfo``, ``ProcessRecord.info``**.  
&nbsp;  
