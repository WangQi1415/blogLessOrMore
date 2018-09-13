---
layout: post
title:  "字符串复习与强化"
date:   2018-09-12 14:30:01 +0800
categories: Python
tag: Python
---

* content
{:toc}


### 1、对齐
```
# 左对齐
string1 = 'cat'
string2 ='catcat'
string3='catcatcat'
width = 7


# print left justified string
print(string1.ljust(width,'c'))  #第二个字符指定空的部分的填充字符，默认为空格
print(string2.ljust(width))
print(string3.ljust(width)) #如果width小于字符串的长度，则会返回原始的字符串，而不是裁段它

```

### 2、子串搜索

```
#子字符串搜索测试
mystr='xxxSPAMxxx'
print('SPAM' in mystr) #True
print('Ni' in mystr) #False
```
### 3、查找
```
#查找
print(mystr.find('SPAM')) #3  返回首个匹配的位置偏移
print(mystr.find('Ni')) #-1 找不到返回-1
```

### 4、替换
```
#替换
mystr.replace('SPAM','aa')
print(mystr) #xxxSPAMxxx  因为字符串是不可变的 所以原字符串并没有发生改变
print(mystr.replace('SPAM','aa')) #而是返回新的字符串
```

### 5、 分割
```
#分割
mystr='\t Ni\n'
print(mystr.strip())  #Ni   去除空白分隔符
print(mystr.rstrip()) #同上，只不过是去除右侧的空白字符
mystr='aaa,bbb,ccc'

mylist=mystr.split(',')#以，为分割，分割为子字符串组成的列表
print(mylist)

mystr='a b\nc\nd'
print(mystr.split()) #默认分隔符：泛空格符
```

### 6、大小写转换
```
#大小写转换
mystr='SHRUBBERY'
print(mystr.lower())
```

### 7、内容测试
```
#内容测试
print(mystr.isdigit())#False
print(mystr.isalpha())#True

```

### 8、连接
```
#连接
delim='NI'
print(delim.join(['aaa','bbb','ccc']))  #连接子字符串列表  aaaNIbbbNIccc
print(' '.join(['A','dead','parrot'])) #在其间添加空格符
```

### 9、类型转换
```
#转换
chars=list('Lorreta') #转换为字符组成的列表
print(chars)
chars.append('!')
str=''.join(chars)  #生成字符串，分隔符为空
print(str) #Lorreta!
```
