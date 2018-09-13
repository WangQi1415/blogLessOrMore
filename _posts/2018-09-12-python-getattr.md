---
layout: post
title:  "Python——走进OOP"
date:   2018-09-09 14:30:01 +0800
categories: Python
tag: Python
---

* content
{:toc}


上节已经提到，使用字典来保存人员信息会导致没有一个集中的地方来存放记录处理的逻辑，这对于小的程序是可以的，但是如果你需要对姓名和涨薪方式稍作改变，可能要在很多地方做改变，项目变大时这绝对是一个体力活。因此我们需要将这些人员的记录和相关的操作方法封装成一个类。

### 如何封装成类
<pre>
class Person:
    def __init__(self,name,age,pay=0,job=None):
        self.name=name
        self.pay=pay
        self.age=age
        self.job=job
    #添加类的方法
    def lastName(self):
        return self.name.split()[-1]
    def giveRaise(self,percent):
        self.pay*=(1.0+percent)
if __name__=='__main__':
    bob=Person('Bob Smith',42,30000,'software')
    sue=Person('Sue Jones',45,40000,'hardware')
    print(bob.name.split()[-1])
    sue.pay*=1.10
    print(sue.pay)
    people=[bob,sue]
    for person in people:
        print(person.name,person.pay)
    x=[rec.name for rec in people if rec.age>=45]
    print(x)

    print(sue.lastName())
    bob.giveRaise(.20)
    print(bob.pay)

</pre>

### 类的继承
我们也可以通过根据需要继承该类并重写该类中的方法
<pre>

from person_start import  Person
class Manager(Person):

    #重写了 giveRaise方法
    def giveRaise(self,percent,bonus=0.1):
        self.pay*=(1.0+percent+bonus)

if __name__=='__main__':
    tom=Manager(name='Tom Doe',age=50,pay=30000)

    bob=Person('Bob Smith',42,10000,'software')
    sue=Person('Sue Jones',45,20000,'hardware')
    print(tom.lastName())
    #tom.giveRaise(.20)
    print(tom.pay)

    people=[bob,sue,tom]
    #多态的实现  以下代码实现了调用giveRaise方法时取决于被处理的对象 obj

    for obj in people:
        obj.giveRaise(.10)
    for obj in people:
        print(obj.lastName(),'=>',obj.pay)
</pre>

### 增加持久化

1、 保存至文件

<pre>
import shelve
from person_start import Person
from manager import Manager

bob=Person('Bob Smith',42,30000,'soft ware')
sue=Person('Sue Jones',45,40000,'hardware')
tom=Manager('Tom Doe',50,50000)

db=shelve.open('class-shelve')
db['bob']=bob
db['sue']=sue
db['tom']=tom
db.close()
</pre>

2、从文件读取

<pre>
import  shelve
db=shelve.open('class-shelve')
for key in db:
    print(key,'=>\n',db[key].name,db[key].pay)
bob=db['bob']
print(bob.lastName())
print(db['tom'].lastName())
</pre>

注意这里不需要重新导入Person类让shelve获取它的实例或者运行它的方法，当实例存储到shelve或者pickle中时，底层的pickle系统会记录下实例的属性以及足够的信息，以便被读取时自动定位到它的类。

3、 进行更新操作
<pre>
import  shelve
db=shelve.open('class-shelve')
sue=db['sue']
sue.giveRaise(.25)
db['sue']=sue

tom=db['tom']
tom.giveRaise(.20)
db['tom']=tom
db.close()


db=shelve.open('class-shelve')
for key in db:
    print(key,"=>\n",db[key].pay)
db.close()
</pre>