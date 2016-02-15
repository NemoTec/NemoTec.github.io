---
layout: post
title: 【AMS】(一)Instrumentation类解析
category: 技术
tags: Android源码
keywords: Instrumentation, Android源码, AMS, android.app.Instrumentation
description: Android源码 android.app.Instrumentation类源码解析，及Instrumentation类在组件生命周期中的作用。
---

注：转载请注明来自Nemo, http://nemotec.github.io  
&nbsp;  
&nbsp;  

### 一、包名
包名：``android.app.Instrumentation``  
父类：无  
子类：无  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这里研究AMS中的Instrumentation类，当然在PMS的PackagePaser中也有同名的内部类(还有Activity, Service, Provider等)，它们是PMS用于解析应用应用的Manifest文件的中间类，不在本次讨论之列。  
&nbsp;  

### 二、概述        
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Instrument, 仪器，乐器，工具。Instrumentation，仪器化，使用仪器。Android源码是google工程师用英文写的，所以对于每一个类的命名也许只有身处他们的语言环境下才真正理解他们所要表达的含义，经过翻译也许能帮我们稍微体会一点他们当时命名这个类的意思。  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Android文档中对Instrumentation类的描述：  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Base class for implementing application instrumentation code.  When running with instrumentation turned on, this class will be instantiated for you before any of the application code, allowing you to monitor all of the interaction the system has with the application.  An Instrumentation implementation is described to the system through an AndroidManifest.xml's &lt;instrumentation&gt; tag.  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Instrumentation类会在应用的任何代码执行前被实列化，用来监控系统与应用的交互。可在以应用的AndroidManifest.xml中<instrumentation>标签来注册一个Instrumentation的实现。  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Java是面向对象的编程语言，每一个类都是一现实世界的一类对象的特征描述，所以粗略地过一下Instrumentation.java代码实现，先在大脑里对它有一个整体的印象，然后在对该类的成员变量细看一下，可以知道这个类的规模，最后浏览一遍所有的方法，就可以大致这个类可能的特征和行为。  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Instrumentation类没继承任何父类，也没实现任何接口，这样也比较好理解。  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Instrumentation另一个重要作用是提供Android组件单元测试。  
&nbsp;  

### 三、java.lang.instrument.Instrumentation       
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;利用Java代码，即java.lang.instrument做动态Instrumentation是 Java SE 5的新特性，它把Java的instrument功能从本地代码中解放出来，使之可以用Java代码的方式解决问题。使用Instrumentation，开发者可以构建一个独立于应用程序的代理程序(Agent)，用来监测和协助运行在JVM上的程序，甚至能够替换和修改某些类的定义。有了这样的功能，开发者就可以实现更为灵活的运行时虚拟机监控和Java类操作了，这样的特性实际上提供了一种虚拟机级别支持的 AOP 实现方式，使得开发者无需对JDK做任何升级和改动，就可以实现某些 AOP 的功能了。  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在Java SE 6里面，instrumentation包被赋予了更强大的功能：启动后的instrument、本地代码(native code)instrument，以及动态改变classpath等等。这些改变，意味着Java具有了更强的动态控制、解释能力，它使得Java语言变得更加灵活多变。  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在 Java SE 6里面，最大的改变使运行时的Instrumentation成为可能。  
在Java SE 5中，Instrument要求在运行前利用命令行参数或者系统参数来设置代理类，在实际的运行之中，虚拟机在初始化之时（在绝大多数的Java类库被载入之前），instrumentation的设置已经启动，并在虚拟机中设置了回调函数，检测特定类的加载情况，并完成实际工作。但是在实际的很多的情况下，我们没有办法在虚拟机启动之时就为其设定代理，这样实际上限制了instrument的应用。而Java SE 6的新特性改变了这种情况，通过Java Tool API中的attach方式，我们可以很方便地在运行过程中动态地设置加载代理类，以达到instrumentation的目的。  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;另外，对native的Instrumentation也是 Java SE 6的一个崭新的功能，这使以前无法完成的功能 —— 对native接口的instrumentation可以在Java SE 6中，通过一个或者一系列的prefix添加而得以完成。  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;最后，Java SE 6里的Instrumentation也增加了动态添加classpath的功能。所有这些新的功能，都使得instrument包的功能更加丰富，从而使Java语言本身更加强大。  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Instrumentation 的最大作用，就是类定义动态改变和操作。在Java SE 5及其后续版本当中，开发者可以在一个普通Java程序(带有main函数的Java类)运行时，通过–javaagent参数指定一个特定的jar文件(包含Instrumentation代理)来启动Instrumentation的代理程序。


### 四、源码解析
#### 1. Activity.java  
1.1 成员mInstrumentation初始化  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Activity类有一个私有Instrumentation成员mInstrumentation，它只在Activity的attach()方法被赋值初始化。  

```
// set by the thread after the constructor and before onCreate(Bundle savedInstanceState) is called.
private Instrumentation mInstrumentation;
......
【ActivityThread】[performLaunchActivity()]
 ↓
【Activity】[attach()]
 ↓
 mInstrumentation = instr;
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;前面分析过从一个Activity子类A通过startActivity()方法启动另一个Activity, 最后就是走到了``ActivityThread.performLaunchActivity()``方法，在这里通过ClassLoader找到要启动的Activity子类B，然后调用Activity子类B的attach()方法给Activity子类B赋以mInstrumentation。  
&nbsp;  

1.2 startActivityForResult()  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Activity有几个类似的启动另一个Activity的方法startActivityForResult()一类方法，都是通过``Instrumentation.execStartActivity()``方法，并返回结果：  

```
Instrumentation.ActivityResult ar =  mInstrumentation.execStartActivity();

【Activity】[startActivity()]  
 ↓  
【Activity】[startActivityForResult()]  
 ↓ 
【Instrumentation】[execStartActivity()]  
......
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;从Activity的启动流程看，这个函数执行比较早的，这里的mInstrumentation要与上面的区分开来，这里是已启动的Activity子类A中的, 它已被初始化，而后面经attach后初始化的是Activity子类B的mInstrumentation。  
&nbsp;  

1.3 performStart(), performRestart(), performResume(), performStop()  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Activity的这四个方法会去调用 mInstrumentation的``callActivityOnStart()``, ``callActivityOnRestart()``, ``callActivityOnResume()``, ``callActivityOnStop()``, 最终Activity的这四个方法会在ActivityThread对应的performXXXActivity()方法中被调用。  

```
【ActivityThread】[performLaunchActivity()]
 ↓
【Activity】[performStart()]
 ↓
【Instrumentation】[callActivityOnStart()]
 ↓
【Activity】[onStart()]
 ......
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;此处mInstrumentation是被启动的Activity子类B经attach后成员变量。  
&nbsp;  

#### 2. ActivityThread.java  
2.1 成员mInstrumentation初始化  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ActivityThread类成员mInstrumentation，它初始化的地方有三个：  

(1) attach(boolean system)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;此处system为true，表示创建的是SystemServer进程时，构造一个空的Instrumentation进行赋值, 并初始化SystemContext.  

```
【SystemServer】[main()]
 ↓
【SystemServer】[run()]
 ↓
【SystemServer】[createSystemContext()]
 ↓
【ActivityThread】[systemMain()]
 ↓
【ActivityThread】[attach(true)]
 ......
```  
&nbsp;  

(2) handleBindApplication(), [data.instrumentationName != null]  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;组件显示调用startInstrumentation()方法才走到这里。此时会通过ClassLoader的loadClass()找到该Class，并实例化赋值给mInstrumentation，并通过init()进行初始化。(此处要看是否有标签<instrumentation>)  

```
......
【ActivityManagerService】[startProcessLocked()]
 ↓
【Process】[start("android.app.ActivityThread", ...)]
 ↓
【ActivityThread】[main()]
 ↓
【ActivityThread】[attach(false)]
 ↓
【ActivityManagerService】[attachApplication()]
 ↓
【ActivityManagerService】[attachApplicationLocked()]
 ↓
【ActivityThread.ApplicationThread】[bindApplication()]
 ↓
【ActivityThread】[handleBindApplication()]
```  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这里再跟踪哪里给data.instrumentationName赋值，instrumentationName由ActivityThread. ApplicationThread的bindApplication()方法传入，它在ActivityManagerService的attachApplicationLocked()方法中被调用，该方法传入参数为app.instrumentationClass，app为ProcessRecord对象，其内部没有给成员instrumentationClass赋值的地方，故只在外部直接赋值，经搜索在ActivityManagerService的startInstrumentation()方法，最终可回溯到ContextImpl的startInstrumentation()方法，而我们知道ContextImpl就是ContextWraper的实际成员mBase,startInstrumentation()最终是在四大组件中在代码里显示调用的，例如：  

```
MainActivity.this.startInstrumentation(new ComponentName("com.settings.test","android.test.InstrumentationTestRunner"), null, null);  
    <instrumentation android:targetPackage="com.android.settings"
        android:name="android.test.InstrumentationTestRunner" />
```  
&nbsp;  

(3) handleBindApplication(), [data.instrumentationName为null]  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;构造APP唯一的Instrumentation.(所有应用第一次创建会走到这里)由(2)中的流程分析，一个应用进程在第一次启动时会创建一个ActivityThread，最终会在handleBindApplication()创建一个空的Instrumentation赋给ActivityThread的成员变量mInstrumentation，后续每一个Activity在启动时经过attach()方法，都会把该mInstrumentation传入Activity赋给Activity.mInstrumentation，也就是同一个应用进程内，所有Activity共用同一个Instrumentation。  
&nbsp;  

2.2 handleBindApplication()  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;创建APP的ActivityThread, Application对象。用到Instrumentation的newApplication(), callApplicationOnCreate()。  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这个方法是ActivityThread创建时调用的，从前面我们知道当前app共用的一个Instrumentation也是在这里创建。紧接着就是最重要部分该app的Application创建:``Application app = data.info.makeApplication(data.restrictedBackupMode, null);``这里data.info是一个LoadedApk对象，它是对应一个apk文件所有信息的，后面研究。这里看看LoadedApk.makeApplication(boolean, Instrumentation)方法：  

```
public Application makeApplication(boolean forceDefaultAppClass, Instrumentation instrumentation) {
    if (mApplication != null) {
        return mApplication;
    }
    
    Application app = null;
    String appClass = mApplicationInfo.className;
    ......
    try {
        java.lang.ClassLoader cl = getClassLoader();
        if (!mPackageName.equals("android")) {
            initializeJavaContextClassLoader();
        }
        ContextImpl appContext = ContextImpl.createAppContext(mActivityThread, this);
        app = mActivityThread.mInstrumentation.newApplication(cl, appClass, appContext);
        appContext.setOuterContext(app);
    } catch (Exception e) {
        ......
    }
    
    mActivityThread.mAllApplications.add(app);
    mApplication = app;
    if (instrumentation != null) {
        try {
            instrumentation.callApplicationOnCreate(app);
        } catch (Exception e) {
            ......
        }
    }
}
```  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;首先，通过mActivityThread.mInstrumentation.newApplication()来创建一个Application实例，过程也是通过本地的ClassLoader对象loadClass()方法找到对应Class，再newInstance()。如果传入的Instrumentation不为空，就通过它的callApplicationOnCreate()方法调用当前app的Application的onCreate()方法，这里传入的是Instrumentation为null，在handleBindApplication()中会显示调用mInstrumentation的callApplicationOnCreate()。这样当前app的Application就准备好了，并在LoadedApk中会保存到mApplication。注意LoadedApk.makeApplication()第一句如果mApplication不为null，就会直接返回mApplication。这样经过第一次创建，后面要newApplication()就直接返回已创建的，所以一个app只有一个Application对象。
&nbsp;  

2.3 performLaunchActivity()  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;startActivity()最后执行的地方。创建要启动的Activity，并调用Activity.attach()方法传入Instrumentation及Application。用到Instrumentation的newActivity()，callActivityOnCreate().  

```  
private Activity performLaunchActivity(ActivityClientRecord r, Intent customIntent) {
    Activity activity = null;
    try {
        java.lang.ClassLoader cl = r.packageInfo.getClassLoader();
        activity = mInstrumentation.newActivity(cl, component.getClassName(), r.intent);
        ......
    } catch (Exception e) {
    
    }
    
    Application app = r.packageInfo.makeApplication(false, mInstrumentation);
    ......
    activity.attach(appContext, this, getInstrumentation(), r.token, 
            r.ident, app, r.intent, r.activityInfo, title, r.parent,r.embeddedID, 
            r.lastNonConfigurationInstances, config, r.voiceInteractor);
    ......
    
    if (r.isPersistable()) {
        mInstrumentation.callActivityOnCreate(activity, r.state, r.persistentState);
    } else {
        mInstrumentation.callActivityOnCreate(activity, r.state);
    }
}
```  
&nbsp;  

2.4 handleReceiver()  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sendBroadcast()最后执行的地方。创建BroadcastReceiver对象，也是通过ClassLoader找到对应的BroadcastReceiver的Class并newInstance()，然后调用``receiver.onReceive()``。这里也会调用到`` LoadedApk.makeApplication(boolean, Instrumentation)``方法，如果当前APP的Application对象还没有创建，会通过``mActivityThread.mInstrumentation.newApplication()``来创建一个Application实例。  

```
private void handleReceiver(ReceiverData data) {
    BroadcastReceiver receiver;
    try {
        java.lang.ClassLoader cl = packageInfo.getClassLoader();
        data.intent.setExtrasClassLoader(cl);
        data.intent.prepareToEnterProcess();
        data.setExtrasClassLoader(cl);
        receiver = (BroadcastReceiver)cl.loadClass(component).newInstance();
    } catch (Exception e) {
        ......
    }

    try {
        Application app = packageInfo.makeApplication(false, mInstrumentation);

        ContextImpl context = (ContextImpl)app.getBaseContext();
        sCurrentBroadcastIntent.set(data.intent);
        receiver.setPendingResult(data);
        receiver.onReceive(context.getReceiverRestrictedContext(), data.intent);
    } catch (Exception e) {
        ......
    }
}
```  
&nbsp;  

 [第一次创建ActivityThread时]  
 
```
【ActivityManagerService】[attachApplication()]
 ↓
【ActivityManagerService】[attachApplicationLocked()]
 ↓
【ActivityManagerService】[sendPendingBroadcastsLocked()]
 ↓
【BroadcastQueue】[sendPendingBroadcastsLocked()]
 ↓
【BroadcastQueue】[processCurBroadcastLocked()]----
 ↓
【ActivityThread.ApplicationThread】[scheduleReceiver()]
 ↓
【ActivityThread】[handleReceiver()]
```  
&nbsp;  

[从四大组件中发送一个广播]  

```
【contextImpl】[sendBroadcast()]
  ↓
【ActivityManagerService】[broadcastIntent()]
  ↓
【ActivityManagerService】[broadcastIntentLocked()]
  ↓
【BroadcastQueue】[scheduleBroadcastsLocked()]
  ↓
【BroadcastQueue】[processNextBroadcast()](很多地方会调用此函数)
  ↓
【BroadcastQueue】[processCurBroadcastLocked()]----
  ↓
【ActivityThread.ApplicationThread】[scheduleReceiver()]
  ↓
【ActivityThread】[handleReceiver()]
```  
&nbsp;  

2.5 handleCreateService()  startService()最后执行的地方。  
 [第一次创建ActivityThread时]  
 
```
【ActivityManagerService】[attachApplication()]
 ↓
【ActivityManagerService】[attachApplicationLocked()]
 ↓
【ActiveServices】[attachApplicationLocked()]
 ↓
【ActiveServices】[realStartServiceLocked()]---
 ↓
【ActivityThread.ApplicationThread】[scheduleCreateService()]
 ↓
【ActivityThread】[handleCreateService()]
```  
&nbsp;  

[从四大组件中启动一个Service]  

```  
【contextImpl】[startService()]
 ↓
【contextImpl】[startServiceCommon()]
 ↓
【ActivityManagerService】[startService()]
 ↓
【ActiveServices】[startServiceLocked()]
 ↓
【ActiveServices】[startServiceInnerLocked()]
 ↓
【ActiveServices】[bringUpServiceLocked()]
 ↓
【ActiveServices】[realStartServiceLocked()]----
 ↓
【ActivityThread.ApplicationThread】[scheduleCreateService()]
 ↓
【ActivityThread】[handleCreateService()]
```  
&nbsp;  

2.6 installProvider()  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Context.getContentResolver().query()最后执行的地方。  

```  
【ContentResolver】[query()][insert()][delete()][update()]
 ↓
【ContextImpl.ApplicationContentResolver】[acquireProvider()]
 ↓
【ActivityThread】[acquireProvider()]
 ↓
【ActivityManagerService】[getContentProvider()]
 ↓
【ActivityManagerService】[getContentProviderImpl()]
 ↓
【ActivityThread.ApplicationThread】[scheduleInstallProvider()]
 ↓
【ActivityThread】[handleInstallProvider()]
 ↓
【ActivityThread】[installContentProviders()]
 ↓
【ActivityThread】[installProvider()]
```  
&nbsp;  

Activity启动流程:  

```
【Activity】[startActivity()]
 ↓
【Activity】[startActivityForResult()]
 ↓
【Instrumentation】[execStartActivity()]
 ↓
【ActivityManagerProxy】[startActivity()]
 ↓
【ActivityManagerNative】[onTransact()]
 ↓
【ActivityManagerService】[startActivity()]
 ↓
【ActivityManagerService】[startActivityAsUser()].
 ↓
【ActivityStackSupervisor】[startActivityMayWait()]
 ↓
【ActivityStackSupervisor】[startActivityLocked()]
 ↓
【ActivityStackSupervisor】[startActivityUncheckedLocked()]
 ↓
【ActivityStack】[startActivityLocked()]
 ↓
【ActivityStackSupervisor】[resumeTopActivitiesLocked()]
 ↓
【ActivityStack】[resumeTopActivityLocked()]
 ↓
【ActivityStack】[resumeTopActivityInnerLocked()]
 ↓
【ActivityStackSupervisor】[startSpecificActivityLocked()](这里如果是Activity第一次启动，会走到【ActivityManagerService】[startProcessLocked()] 创建进程。)
 ↓
【ActivityStackSupervisor】[realStartActivityLocked()]
 ↓
【ActivityThread.ApplicationThread】[scheduleLaunchActivity()]
 ↓
【ActivityThread】[handleLaunchActivity()]
 ↓
【ActivityThread】[performLaunchActivity()]
 ↓
【Instrumentation】[newActivity()]
 ↓
【Instrumentation】[callActivityOnCreate()]
 ↓
【Activity】[OnCreate()]
```  
&nbsp;  

### 五、总结  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Instrumentation只在Activity和ActivityThread中有实例。一个APP启动时会创建一个ActivityThread，会在ActivityThread的handleBindApplication()创建一个空的Instrumentation对象并保存在ActivityThread的成员变量mInstrumentation，后续每一个Activity在启动时经过Activity的attach()方法，都会把ActivityThread的mInstrumentation赋给Activity的成员mInstrumentation，也就是同一个应用进程内，所有Activity共用ActivityThread中的同一个Instrumentation。每一个APP的Application对像也是在handleBindApplication()中经由Instrumentation的newApplication()方法创建，且只有一个。  

1.每一个应用进程中只有唯一的ActivityThread, (ActivityThread中，成员【private static ActivityThread sCurrentActivityThread】, 方法【public static ActivityThread currentActivityThread()】)  

2.每一个应用进程中只有唯一的Application, (ActivityThread中，成员【Application mInitialApplication】, 方法【public Application getApplication()】)  

3.每一个应用进程中只有唯一的Instrumentation, (ActivityThread中，成员【Instrumentation mInstrumentation】, 方法【public Instrumentation getInstrumentation()】)  

4.四大组件第一次启动时都会通过LoadedApk中的getClassLoader()方法，创建当前apk的PathClassLoader对象，后续每次要启组新的组件都会通过该ClassLoader来得到组件实际的Class。  
&nbsp;  

进程中某类的实例唯一性判断：  
(1) 一个静态单例模式的类。  
(2) 只被new或Class.newInstance()一次的成员。  
(3) Java赋值"="的按值传递特性，而定义的实例名称实际是指向一个地址，所以不论进行多少次的"="赋值，只要没有new操作，各变量名实际都是指向同一个实例。
