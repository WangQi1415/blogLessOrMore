---
layout: post
title:  "Python中的 getattr()"
date:   2018-09-12 14:30:01 +0800
categories: Python
tag: Python
---

* content
{:toc}


### 方法摘要：
getattr()用来返回对象的命名属性的值，若不存在则返回默认值
### 参数
有多个参数  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1、 object - 要返回其命名属性值的对象  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2、 name - 包含属性名称的字符串  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3、 default (Optional) - 找不到命名属性时返回的值  
### 返回值：
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1、 给定对象的命名属性的值  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2、 如果未找到命名属性则返回 default设置的值  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3、 如果未找到命名属性且未定义默认值，则报AttributeError exception  
### 代码测试：
<pre>
class Person:
    age = 23
    name = "Adam"

person = Person()
print('The age is:', getattr(person, "age"))  #The age is: 23
print('The name is',getattr(person,"name"))  #The name is Adam
print('The sex is',getattr(person,"sex"))  #AttributeError: 'Person' object has no attribute 'sex'
print('The sex is',getattr(person,"sex","not found"))  #The sex is not found
print('The age is:', person.age)
</pre>