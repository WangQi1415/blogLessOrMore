---
layout: post
title:  "__str__与__dict__,及字符串与dict之间的转换"
date:   2018-09-09 11:40:01 +0800
categories: Python
tag: Python
---

* content
{:toc}


当打印一个类的实例化对象的时候 会调用  \_\_str\_\_  方法  
\_\_dict\_\_   保存了实例化对象的属性  
字符串转 list，dict等内置对象 使用 eval()方法  
内置对象转 list、dict等 使用str()方法  

代码测试：  
<pre>
class strtest:
    def __init__(self,name,age):
        self.name=name
        self.age=age
        print("init: this is only test")

    def __str__(self):
        #字典转字符串
        return str(self.__dict__)


if __name__ == "__main__":
    st = strtest('zhangsan',18)

    #当打印一个类的实例化对象的时候 会调用  __str__方法
    print(st)  #{'name': 'zhangsan', 'age': 18}
    print(st.__dict__)   #{'name': 'zhangsan', 'age': 18}  将实例中的所有属性均打印出来

    #字符串与字典之间的相互转换
    mystr="{'name': 'zhangsan', 'age': 18}"
    mystr1="['name','zhangsan', 'age',18]"
    mydict=eval(mystr)
    print(type(mydict),mydict)

    mylist=eval(mystr1)
    print(type(mylist),mylist)

</pre>



