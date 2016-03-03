---
layout: post
title: 【设计原则】面向对象六大原则之一：开闭原则
category: 架构
tags: 设计原则
keywords: 开闭原则
description: 介绍面向对象设计中的六大原则之一：开闭原则。对扩展开放，对修改关闭。
---
注：转载请注明来自Nemo, http://nemotec.github.io  
&nbsp;  
&nbsp;  


**开闭原则(OCP)**  
&nbsp;  
【全称】 "Open-Closed Principle" 开放－封闭原则  
&nbsp;  
【说明】 软件实体如类、模块、函数，应该 **``对扩展开放，对修改关闭``**。即软件实体应尽量在不修改原有代码的情况下进行扩展。  
&nbsp;  
【实现】  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在面向对象设计中，**``不允许更改的是系统的抽象层，而允许扩展的是系统的实现层``**。换言之，定义一个一劳永逸的抽象设计层，允许尽可能多的行为在实现层被实现。  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;解决问题关键在于抽象化，抽象化是面向对象设计的第一个核心本质。**``对一个事物抽象化，实质上是在概括归纳总结它的本质``**。抽象让我们抓住最最重要的东西，从更高一层去思考。这降低了思考的复杂度，我们不用同时考虑那么多的东西。换言之，我们封装了事物的本质，看不到任何细节。  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在面向对象编程中，**``通过抽象类及接口，规定了具体类的特征作为抽象层，相对稳定，不需更改，从而满足“对修改关闭”；而从抽象类导出的具体类可以改变系统的行为，从而满足“对扩展开放”。``** 最后实现效果：1、要能够复用；2、扩展时只增加新方法、新类；3、不得不修改代码时，修改的范围必须是局部的、隐藏的；  
&nbsp;  
【优点】  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;按照OCP原则设计出来的系统，降低了程序各部分之间的耦合性，其适应性、灵活性、稳定性都比较好。当已有软件系统需要增加新的功能时，**``不需要对作为系统基础的抽象层进行修改，只需要在原有基础上附加新的模块就能实现所需要添加的功能``**。增加的新模块对原有的模块完全没有影响或影响很小，这样就无须为原有模块进行重新测试。  
&nbsp;  
【示例】  

##### 1.定义函数的参数类型、引用对象尽量使用接口或者抽象类，而不是实现类：  

```
public String getString(ArrayList<String> list) {
    ....
}

public String getString(LinkedList<String> list) {
    ....
}
```  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对于上面函数的参数类型，其实我们可以使用更具抽象的List来代替ArrayList、LinkedList，由于它们实现了接口List<E>，我们使用接口类更具一般性，避免了重复代码：  

```
public String getString(List<String> list) {
    ....
}
```  

##### 2.通过接口或者抽象类约束扩展，对扩展进行边界限定，不允许出现在接口或抽象类中不存在的public方法；  

```
public interface IScanner {
    public void scan();
    public void cancelScan();
}

public interface IUpdater {
    public void check();
    public void cancelCheck();
    public void update();
    public void cancelUpdate();
}

public class ScannerImpl {
    public void scan() {
        ....
    }
    
    public void cancelScan() {
        ....
    }
}

public class VirusEngine {
    private IScanner mScanner;
    private IUpdater mUpdater;
    ....

    private init() {
        mScanner = new ScannerImpl();
        ....
    }
}
```  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这里对封装变化也有要求：第一，将相同的变化封装到一个接口或者抽象类中；第二，将不同的变化封装到不同的接口或抽象类中，不应该有两个不同的变化出现在同一个接口或抽象类中。   

##### 3.抽象化  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;抽象层尽量保持稳定，一旦确定即不允许修改。可以看出抽象化在面向对象设计中的重要性，需要深刻理解业务的本质，把系统的所有可能的行为抽象成一个抽象底层，这个抽象底层规定出所有的具体实现必须提供的方法的特征。作为系统设计的抽象层，要预见所有可能的扩展，从而使得在任何扩展情况下，系统的抽象底层不需修改；同时，由于可以从抽象底层导出一个或多个新的具体实现，可以改变系统的行为，因此系统设计对扩展是开放的。  

##### 4.制定项目章程  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在一个团队中，建立项目章程是非常重要的，因为章程中指定了所有人员都必须遵守的约定，对项目来说，约定优于配置。


