---
layout: post
title:  "Python 中的map()"
date:   2018-08-29 11:40:01 +0800
categories: Python学习
tag: map
---

* content
{:toc}



### Python中的 map()函数
#### 1、语法
map(function, iterable, ...)
#### 2、参数详情
function:将集合的每个元素当做参数传递给该函数
iterable: 可迭代的集合，列表，元组等

#### 3、返回值
 map()的返回值是一个map()对象  可以使用list(),set()函数来创建列表或者集合
 
 示例：  
```
def calculateSquare(n):
  return n*n

numbers = (1, 2, 3, 4)
result = map(calculateSquare, numbers)
print(result)
# converting map object to set
numbersSquare = set(result)
print(numbersSquare)

```
输出：  

```
<map object at 0x7fcd4b65f908>
{16, 1, 4, 9}
```

#### 4、map()与lambda 搭配使用
lambda函数经常与 map()搭配使用，来使得代码变得更加的简洁

````
numbers = (1, 2, 3, 4)
result = map(lambda x: x*x, numbers)
print(result)

# converting map object to set
numbersSquare = set(result)
print(numbersSquare)

结果：
<map object at 0x7fcd4b65fa20>
{16, 1, 4, 9}
````
#### 5、可以传递多个集合的元素作为参数

```
num1 = [4, 5, 6]
num2 = [5, 6, 7]

result = map(lambda n1, n2: n1+n2, num1, num2)
print(list(result))

结果：
[9, 11, 13]
```