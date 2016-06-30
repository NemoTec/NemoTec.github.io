---
layout: post
title: 【工具】(二) OpenGrok搜索
category: 技术
tags: 工具
keywords: OpenGrok
description: OpenGrok搜索使用
---
注：本文作者Nemo, http://nemotec.github.io    
&nbsp;  
&nbsp;  


#### **一、网址**  

http://androidxref.com/  

可以快速搜索Android源码的一个网站，响应速度很快。也可以配置本地源码。  
&nbsp;  

#### **二、各搜索行释义**  

**Full Search**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Search through all text tokens(words,strings,identifiers,numbers) in index.  

**Definition**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Only finds symbol definitions.  

**Symbol**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Only finds symbols.  

**File Path**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Path of the source file.  

**History**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;History log comments.  
&nbsp;  

#### **三、搜索技巧**  

**1.""的使用**   

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**对于不可分割的词且两者间包含空格则可使用双引号。**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;此规则适用于所有的搜索规则，例如Full Search中输入"Activity act"，双引号去除表示或的关系。  
&nbsp;  
**2.布尔操作**  

**(1) +表示包含此字符串，-表示包含此字符串。**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在Full Search中输入[+"activity" -"service"]，搜索包含"activity"字符串但是不包含"service"字符串的源文件。  

**(2) 与**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;【AND】, 【&&】, 【+】连接同时包含的字符串  

**(3) 或**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;【OR】, 【||】连接至少包含一个的字符串，或者两个用空格隔开的字符串也表示或关系。

**(4) 非**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;【NOT】, 【!】, 【-】连接不包含的字符串  
&nbsp;  
**3.通配符**  

(1) ?代表一个字符，\*代表多个字符(?和\*不可用于字符串的开头)。  

(2) 使用”~”搜索包含与提供的字符串拼写类似的源码文件等内容。  

(3) 转义字符，OpenGrok 中使用到的特殊字符包括+ - && || ! ( ) { } [ ] ^ " ~ * ? : \ ，因此如果需要搜索的内容中包含这些特殊字符，可以使用\进行转义。  

&nbsp;  
