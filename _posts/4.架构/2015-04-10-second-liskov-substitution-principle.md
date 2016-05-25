---
layout: post
title: 【设计原则】面向对象设计原则之二：里氏替换原则
category: 架构
tags: 设计原则
keywords: 里氏替换原则, 面向对象设计
description: 介绍面向对象设计原则之二：里氏替换原则。所有引用基类的地方必须能够透明的使用其子类对象。
---
注：本文来自Nemo, http://nemotec.github.io  
&nbsp;  
&nbsp;  


#### **里氏替换原则(LSP)**  
&nbsp;  

#### **一、全称**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Liskov Substitution Principle" 里氏替换原则  
&nbsp;  

#### **二、说明**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;``Functions that use pointers or references to base classes must be able to use objects of derived classes without knowing it.``  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;``所有引用基类的地方必须能够透明的使用其子类对象。`` 即只要父类出现的地方子类就能够出现，而且替换为子类不会产生任何错误或异常。但是反过来，子类出现的地方，替换为父类就可能出现问题了。这项原则最早是在1988年，由麻省理工学院的一位姓里的女士（Barbara Liskov）提出来的。  
&nbsp;  

#### **三、实现**  

**1. 子类可以实现父类的抽象方法，但不能（或者尽量不要）覆盖父类的非抽象方法**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;从语法上讲，覆盖父类的非抽象方法是不会报错的，但这种做法本身是违背了面向对象抽象设计原则，因为父类实现了该方法，就是做为一类实例的整体抽象，子类该是继承这种抽象行为，而不是重写。如果是子类特有的，应该做为子类自己的行为方法来定义。  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;由里氏替换原则的定义，所有使用父类的地方都可以用子类替换。如果我们用多态的原则使用父类的引用指向子类，那么因子类已重新实现了该方法而失去了父类本来想要表达的行为方法。  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;如果非要重写父类的方法，比较通用的做法是：``原来的父类和子类都继承一个更通俗的基类，原有的继承关系去掉，采用依赖、聚合，组合等关系代替。``  
&nbsp;  
**2. 子类可以有自己的特性**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;子类可以定义其它的成员、方法，来表达自己不同于父类的特性。  
&nbsp;  
**3. 覆盖或实现父类的方法时输入参数要被放大**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当子类重载父类的方法时，方法的前置条件（即方法的形参）要比父类方法的输入参数更宽松。同样用多态的方法来考虑这一条，如果用父类的引用来达到抽象效果，它直接调用了某一方法，我们是不知道将来这个引用会指向的子类会如何实现该方法，如果``子类方法输入参数类型是父类方法输入参数类型的基类``，就能保证所有适用于父类的输入数据，都能适用于子类。  
&nbsp;  
**4. 覆盖或实现父类的方法时输出结果要被缩小**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当子类的方法实现父类的抽象方法时，方法的后置条件（即方法的返回值）要比父类更严格。这一条也是为在面向对象多态性时，我们事先能知道的返回结果是父类方法返回结果。为保证使用父类的引用指向子类时，通过父类引用调用方法返回的结果也适于这个返回结果，``子类方法返回结果类型应该是父类方法返回结果类型的子类``。  
&nbsp;  

#### **四、优点**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;里氏替换原则的核心原理是抽象，抽象依赖于面向对象继承这个特性，在面向对象设计中，继承的优缺点相当明显。  

优点：  
(1) 代码重用，减少创建类的成本，每个子类都拥有父类的方法和属性；  
(2) 子类与父类基本相似，但又与父类有所区别；  
(3) 提高代码的高扩展性。  

缺点：  
(1) 继承是侵入性的，只要继承就必须拥有父类的所有属性和方法；  
(2) 可能造成子类代码冗余、灵活性降低，因为子类必须拥有父类的属性和方法。  
&nbsp;  

#### **五、示例**  

1.子类重写父类方法，引起抽象对象引用的行为发生未预知的错误：  

```
class Base {
    public int Func(int a, int b){
        return a-b;
    }
}

class A extends Base {
    public int Func(int a, int b){
        return a+b;
    }

    public int Func2(int a, int b){
        return a*b;
    }
}

public class Client{
    public static void main(String[] args){
        Base base = new Base();
        System.out.println("200-100=" + base.Func1(200, 100));

        base = new A();
        System.out.println("200-100=" + base.Func1(200, 100));
    }
} 
```  

&nbsp;  
2.子类方法参数类型比父类方法类型更严，编译通过但子类方法内部可能发生未预知的错误：  

```
class Base {
    public void setData(Map<String, Object> map){
        ....
    }
}

class A extends Base {
    public void setData(LinkedHashMap<String, Object> map){
        ....
    }

    public int Func2(int a, int b){
        return a*b;
    }
}

public class Client {
    public static void main(String[] args) {  
        HashMap<String, Object> hashMap = new HashMap<String, Object>();
        Base base = new A();
        base.setData(hashMap);
    }
} 
```  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;基类方法参数类型为Map，子类方法参数类型为LinkedHashMap，由于外部是用基类引用指向子类，在编译时是可以通过的，但在运行时调用子类的方法传入的参数为HashMap类型，比原来参数类型LinkedHashMap宽泛，可能报类型不兼容错误。   
&nbsp;  
3.子类覆写基类方法，返回值比父类宽泛，可能导致运行时返回类型不兼容：  

```
class Base {
    public LinkedHashMap<String, Object> getData(){
        ....
    }
}

class A extends Base {
    public Map<String, Object> getData(){
        ....
    }

    public int Func2(int a, int b){
        return a*b;
    }
}

public class Client {
    public static void main(String[] args) {  
        Base base = new A();
        LinkedHashMap<String, Object> hashMap = base.getData();
    }
} 
```  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;基类方法返回类型为LinkedHashMap，子类覆盖方法的返回为Map，则可以传回的为HashMap，这样外部需要的是LinkedHashMap，可能出现类型不兼容问题。  

&nbsp;  
