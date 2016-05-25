---
layout: post
title: 【设计原则】面向对象设计原则之三：依赖倒置原则
category: 架构
tags: 设计原则
keywords: 依赖倒置原则, 面向对象设计
description: 介绍面向对象设计原则之三：依赖倒置原则。依赖倒置原则的核心是面向接口编程。
---
注：本文来自Nemo, http://nemotec.github.io  
&nbsp;  
&nbsp;  


#### **依赖倒置原则(DIP)**  
&nbsp;  

#### **一、全称**   

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Dependence Inversion Principle" 依赖倒置原则  
&nbsp;  

#### **二、说明**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;``High level modules should depend upon low level modules. Both should depend upon abstractions. Abstractions should not depend upon details. Details should depend upon abstractions.``  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.高层模块不应该依赖低层模块，两者都应该依赖于抽象(抽象类或接口)；  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.抽象(抽象类或接口)不应该依赖于细节(具体实现类)；  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.细节(具体实现类)应该依赖抽象。  
&nbsp;  

#### **三、实现**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;依赖倒置原则要求在``依赖关系``或``关联关系``中，尽量引用层次高的抽象层类，使用``接口和抽象类``进行``变量类型声明``、``参数类型声明``、``方法返回类型声明``，以及``数据类型的转换``等，而不要用具体类来做这些事情。为了确保该原则的应用，一个具体类应当``只实现接口或抽象类中声明的方法``，而不要给出多余的方法，否则将无法调用到在子类中增加的新方法。  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;依赖倒置原则的核心思想是**面向接口编程**。相对于细节的多变性，抽象的东西要稳定的多。以抽象为基础搭建起来的架构比以细节为基础搭建起来的架构要稳定的多。使用接口或者抽象类制定好规范和契约，而不涉及任何具体的操作，把展现细节的任务交给他们的实现类去完成。  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在实现依赖倒置原则时，我们需要针对抽象层编程，而将具体类的对象通过依赖注入(Dependency Injection，DI)的方式注入到其他对象中，依赖注入是指当一个对象要与其他对象发生依赖关系时，通过抽象来注入所依赖的对象。在运行时再传入具体类型的对象，由子类对象来覆盖父类对象。  
&nbsp;  
**1. 构造函数传递依赖对象(以抽象类或接口的形式)，即: 构造注入**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;声明的构造函数参数类型为抽象类或接口的形式，运行时传入构造函数为具体类的对象。  
&nbsp;  
**2. setter方法传递依赖对象(以抽象类或接口的形式)，即: 设值注入**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;声明的setter方法参数类型为抽象类或接口的形式，运行时传入setter方法为具体类的对象。  
&nbsp;  
**3. 接口声明实现依赖对象(以抽象类或接口的形式)，即: 接口注入**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;声明的接口内的方法参数类型为抽象类或接口的形式，运行时传入接口内的方法为具体类的对象。  
&nbsp;  

#### **四、优点**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;依赖倒置原则的核心是面向接口编程，其优缺点相当明显。  

优点：  
(1) 接口和实现，业务逻辑清晰，代码易懂，适于团队的协作开发。  
(2) 实现低耦合的系统，便于以后升级扩展，可维护性强。  

缺点：  
(1) 接口设计要考虑全面，否则后续修改接口影响范围很大。  
&nbsp; 
 
#### **五、示例**  

1.构造注入：  

```
public class ScanListenerTms implements IScanListener, IDeleteListener {
    public ScanListenerTms(IViewScan viewInterface) {
        ....
    }
}

public class UpdateListenerTms implements IUpdateListener {
    public UpdateListenerTms(IViewScan viewInterface) {
        ....
    }
}
```  

&nbsp;  
2.设值注入：  

```
class BaseImpl implements IBase {
    public void setRecord(IRecord ScoreInterface) {
        ....
    }
    
    public void setScore(IScore ScoreInterface) {
        ....
    }
}
```  

&nbsp;  
3.接口注入：  

```
interface IScanner {

    public void registerListener(IScanListener scanListener);

    public void unregisterListener();
}

interface IUpdater {

    public void registerListener(IUpdateListener updateListener);

    public void unregisterListener();
}
```  

&nbsp;  

