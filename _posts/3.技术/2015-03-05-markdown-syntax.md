---
layout: post
title: 【工具】(一) Markdown语法技巧
category: 技术
tags: 工具
keywords: Markdown
description: 这篇是个人学习Markdown语法技巧时的总结，用纯文本编辑打开就可以看到各示例内容的Markdown语法实现，基本可以解决最常用的Markdown编辑方法中90%的问题，更深入的研究会在学习后持续更新。
---
注：本文作者Nemo, http://nemotec.github.io  
&nbsp;  
&nbsp;  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这篇是个人学习Markdown语法技巧时的总结，用纯文本编辑打开就可以看到各示例内容的Markdown语法实现，基本可以解决最常用的Markdown编辑方法中90%的问题，更深入的研究会在学习后持续更新。  
&nbsp;  

#### **一、标题**  

【语法】【类Atx形式: 行首1-6个#, 再加1-N个空格, 再加标题。(标题后+N个空格+N个#可有可无)】  
【效果】【类Setext形式: 用 = (最高阶标题) 和 - (第二阶标题)】 
># H1标题     #########
>## H2标题 #######
>### H3标题 ###
>#### H4标题
>##### H5标题
>###### H6标题

>第一级标题
>================
>第二级标题
>-
&nbsp;  

#### **二、粗体**  

【语法】【在文字前后各加\*\*或\_\_】  
【效果】【**这段文字会变粗体1** 或 __这段文字会变粗体2__】  
&nbsp;  

#### **三、斜体**  

【语法】【在文字前后各加\*或\_】  
【效果】【*文字变斜体1* 或 _文字变斜体2_】【~~删除线~~】【++下划线++】【~这是什么~】  
&nbsp;  

#### **四、空行**  

【语法】【一行(非空行)结尾处: 两个空格+换行+空行】  
【效果】【得到一个空行(但在空行结尾两个空格+换行无效，可用\<br/>)】    
&nbsp;  

#### **五、空格**  

【语法】【\&nbsp;】  
【效果】【一个空格】  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;中文行的开关一般都有两个中文字符的空格。中文行的开关一般都有两个中文字符的空格。中文行的开关一般都有两个中文字符的空格。  
&nbsp;  

#### **六、有序列表**  

【语法】【 数字+英文句点+空格, 后接表项内容。  
对于二级或三级有序列表，可在新行前加TAB符或3个及以上的空格。自己多用4个空格。  
一级为1, 2, 3; 二级为i, ii, iii; 三级为a, b, c. 后面再分级均是a, b, c.】  
【效果】【前面的数字可以乱序】  
>1. 有序表项01
>2. 有序表项02
>3. 有序表项03  

以下在Github上好像无效？  
102. 有序表项--01  
2. 有序表项--02  
7. 有序表项--03  

也可以:
1. 一级有序表项1  
    1. 二级有序表项1-1  
        1. 三级有序表项1-1-1  
        2. 三级有序表项1-1-2  
            4. 四四  
                5. 五五  
    2. 二级有序表项1-2  
2. 一级有序表项2  
3. 一级有序表项3  
&nbsp;  

#### **七、无序列表**  

【语法】【*(或+或-)+空格(最多4个), 后接表项内容。  
对于二级或三级无序列表，可在新行前加TAB符或2个及以上的空格。自己多用4个空格。  
一级为实心圆点, 二级为空心圆点, 三级及以上为实心方形。】  
【效果】【】  
>* 无序表项01
>* 无序表项02
>* 无序表项03

>+ 表项11,  
表项长内容1------  
表项长内容2------
>+ 表项12
>+ 表项13

- 一级无序表项1
  - 二级无序表项1-1
    - 三级无序表项1-1-1
    - 三级无序表项1-1-2
      - 四四
        - 五五
  - 二级无序表项1-2
- 无序表项2
- 无序表项3  
&nbsp;  

#### **八、引用**  

【语法】【>】  
【效果】【】 
>一级引用-1
>>二级引用--2
>>>三级引用---3  
>>二级引用--4无效？  
>一级引用-5无效？  

>一级引用-1
>>二级引用--2
>>>三级引用---3
>>
>>二级引用--4
>
>一级引用-5  
&nbsp;  

#### **九、表格**  

【语法】【  
    ------: 为右对齐;  
    :------ 为左对齐;  
    :------: 为居中对齐;  
    ------- 为使用默认居中对齐】  
【效果】【Github上表格好像没有效果？？？】  
| 我的笔记 |  特性 |   结果  |  
|:---------:|-----------:|--------|  
| MyNote 123 | Markdown | OK--------- |  
&nbsp;  

#### **十、链接**  

【语法】【\[描述\]\(链接地址\)】  
【效果】【注意要使用英文符号】  
内联方式: [百度主页](www.baidu.com)  
或引用方式:  
I get 10 times more traffic from [Google][1] than from [Yahoo][2] or [MSN][3].  

[1]: http://google.com/        "Google" 
[2]: http://search.yahoo.com/  "Yahoo Search" 
[3]: http://search.msn.com/    "MSN Search"
&nbsp;  

#### **十一、图片**  

【语法】【\!\[Alt text\](xx/yy/zz.jpg "Optional title")】  
【效果】【  
Alt text 为如果图片无法显示时显示的文字;  
xx/yy/zz.jpg为图片所在路径;  
(1) Optional title 为显示标题。显示效果为在你将鼠标放到图片上后，会显示一个提示框。  
(2) 图片路径可以使用绝对路径也可以使用相对路径，建议使用相对路径。通常的做法是Markdown文档的同级目录下建立一个pictures文件夹，里面放置所有所需的图片。  
(3) **GitHub图片用法: 找到repo中图片，点击图片可获得路径，如
YourGithub/Project/blob/master/Res/ActivityCallBack.png，用Markdown添加图片语法，但是需要注意的是链接中需将blob改为raw。**】  

(1) 内联方式:  

![image](http://mdp.tylingsoft.com/icon.png "PNG")  

(2) 引用方式:  

![alt text][id4]

[id4]: http://mdp.tylingsoft.com/icon.png "Title"

(3) GitHub图片用法:  

![alt text](https://github.com/NemoTec/Picture/raw/master/NotePic/markdown/ActivityCallBack.png)
&nbsp;  

#### **十二、代码块**  

【语法】【使用\`\`\`表示代码块。2个顿点表示行内代码，有的1个或3个也可表示行内代码】  
【效果】【\`是英文下的TAB键上方的键，下例中的javascript可以不写。】  

```javascript
var canvas = document.getElementById("canvas");
if (isOpen) {
    xx.y = 2.4;
}
var context = canvas.getContext("2d");
```

这是``java.lang.String``类的实例。行内代码`ClassLoader`, ```reflect```.  
&nbsp;  

#### **十三、流程图**  

【语法】【】  
【效果】【】  

```  
graph LR  
A-->B
```  
&nbsp;  

<span id = "jumpTo">页内跳转会跳到这吗</span>  
&nbsp;  

#### **十四、时序图**  

【语法】【】  
【效果】【】  
&nbsp;  

#### 十五、Math公式
【语法】【】  
【效果】【】  
&nbsp;  

#### **十六、分隔线**  

【语法】【3个及以上\* 或 \-, 或3个以上\* 加 1个空格】  
【效果】【】  

第一行
----------
第二行
***
第三行
* * ****
&nbsp;  

#### **十七、转义字符**  

【语法】【Markdown 支持一些符号前面加上反斜杠来插入普通的符号】  
【效果】【\\#  --->  # 或 \\_  --->  _】  
\\  反斜线  
\`  反引号  
\*  星号  
\_  底线  
\{} 花括号  
\[] 方括号  
\() 括弧  
\# 井字号  
\+ 加号  
\-  减号  
\.  英文句点  
\!  惊叹号  
\< 左尖括弧  
&nbsp;  

#### **十八、目录**  

【语法】【[TOC]】  
【效果】【】  
&nbsp;  

#### **十九、注脚**  

【语法】【】  
【效果】【】  
这是一个注脚测试[^footer1]。
[^footer1]: 这是一个测试，用来阐释注脚。  
&nbsp;  

#### **二十、页内跳转**  

【语法】【】  
【效果】【】  
[页内跳转测试](#jumpTo)  
&nbsp;  
&nbsp;  

参考:  
[Markdown语法说明](www.appinn.com/markdown)  
[Markdown语法](http://www.tuicool.com/articles/fmeMbqR)  
[流程图](http://adrai.github.io/flowchart.js)  
[时序图](http://bramp.github.io/js-sequence-diagrams)  

&nbsp;  
