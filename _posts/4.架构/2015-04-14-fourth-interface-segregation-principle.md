---
layout: post
title: 【设计原则】面向对象设计原则之四：接口隔离原则
category: 架构
tags: 设计原则
keywords: 接口隔离原则, 面向对象设计
description: 介绍面向对象设计原则之四：接口隔离原则。接口尽量细化。
---
注：本文来自Nemo, http://nemotec.github.io  
&nbsp;  
&nbsp;  


#### **接口隔离原则(ISP)**  
&nbsp;  

#### **【全称】**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Interface Segregation Principle" 接口隔离原则  
&nbsp;  

#### **【说明】**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;``The dependency of one class to another one should depend on the smallest possible interface.``  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;接口隔离原则要求的是在一个模块应该只依赖它需要的接口，而且需要保证接口应该尽量小，即设计接口的时候应该让接口尽量细化，不要定义太臃肿的接口（比如接口中有很多不相干的逻辑的方法声明）。  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;接口隔离原则与单一职责原则有些相似，不过不同在于：单一职责原则要求的是类和接口职责单一，注重的是职责，是业务逻辑上的划分。而接口隔离原则要求的是接口的方法尽量少，尽量有用（针对一个模块）  
&nbsp;  

#### **【实现】**  

**1. 接口尽量小**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;接口尽量小保证一个接口只服务一个子模块或者业务逻辑。  
&nbsp;  
**2. 接口高内聚**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;接口高内聚是对内高度依赖，对外尽可能隔离。即一个接口内部的声明的方法相互之间都与某一个子模块相关，且是这个子模块必须的。  
&nbsp;  
**3. 接口设计是有限度的**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;如果完全遵循接口隔离原则的话，会出现接口的设计力度会越来越小，造成接口数量剧增，系统复杂度一下子增加。所以需要根据经验或者尝试判断，设计合适的接口规模。  
&nbsp;  

#### **【优点】**  

优点：  
(1) 接口细化，增加了代码灵活性和复用性。  
(2) 降低模块间的耦合，便于升级扩展，可维护性强。  

缺点：  
(1) 若接口设计过小，则会造成接口数量过多，使设计复杂化。  
&nbsp;  

#### **【示例】**  

&nbsp;  
