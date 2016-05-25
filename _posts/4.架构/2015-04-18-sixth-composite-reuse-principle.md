---
layout: post
title: 【设计原则】面向对象设计原则之六：合成复用原则
category: 架构
tags: 设计原则
keywords: 合成复用原则, 面向对象设计
description: 介绍面向对象设计原则之六：合成复用原则。多用组合，少用继承。
---
注：本文来自Nemo, http://nemotec.github.io  
&nbsp;  
&nbsp;  


#### **合成复用原则(CRP)**  
&nbsp;  

#### **一、全称**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Composite Reuse Principle" 合成复用原则  
&nbsp;  

#### **二、说明**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;``"Composite/Aggregate Reuse Principle" 合成/聚合复用原则`` 或 ``"Composite Reuse Principle" 合成复用原则``  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;``尽量使用合成/聚合，避免使用继承``。如果新对象的某些功能在别的已经创建好的对象里面已经实现，那么尽量使用别的对象提供的功能，使之成为新对象的一部分，而不要自己再重新创建。新对象通过向这些对象的委派达到复用已有功能的。  
&nbsp;  

#### **三、实现**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当要使用已创建好的某个类的实现，尽量使用组合的关联方式，使之成为当前类的一部分，而不是继承这种强耦合关系，把耦合关系降低。  
&nbsp;  

#### **四、优点**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;里氏替换原则的核心原理是多用组合，少用继承，比较二者的异同点，就很容易看到里氏替换原则的优缺点。  

优点：  
(1) 降低类与类之间的耦合度，一个类的变化对其他类造成的影响相对较少；  
(2) 封装性更好，不用对复用类的内部细节有过多的了解；  
(3) 系统更加灵活，多态性使得可以到运行时再指定引用指向的对象，基类继承是静态的。  
&nbsp;  

#### **五、示例**  

&nbsp;  
