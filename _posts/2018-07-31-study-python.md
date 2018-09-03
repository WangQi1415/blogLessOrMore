---
layout: post
title:  "Python基础"
date:   2018-07-31 14:37:01 +0800
categories: Python
tag: Python基础
---

* content
{:toc}


#### 一、基本数据类型
python3中有六种标准的数据类型：  

1. Numbers  数字
2. String 字符串
3. List 列表
4. Tuple 元祖
5. Sets 集合
6. Dictionaries 字典


##### 1、 数字

```
a,b,c,d,e=20,5.5,True,4+3j,"abc";
print(a,"\t",b,"\t",c,"\t",d,"\t",e);
#可使用type()函数查看变量的数据类型
print(type(a),type(b),type(c),type(d),type(e));

#数值运算（+ - × / 幂  取余 类型转换）
a=2+3;
print(a);
b=50-5*6;
print(b);
c=(50-5*6)/4;
print(c);
d=8/5;
print(d);
#除法总会返回一个浮点数
e=17/3;
print(e);
#如果想保留整数部分  使用 //
f=17//3;  #5
print(f);

g=17%3;
print(g);

#幂运算
h=5**2;
print(h);
#求平方跟  此种方法只能求正数的平方根
sqrt=4**0.5
print('sqrt',sqrt)
#也可使用cmath中的 sqrt()函数来求平方根
import cmath
num_sqrt=cmath.sqrt(-8)
print(num_sqrt)

#类型转换  不同类型的数最终会转换为浮点数
i=3*3.75/1.5;
print(i);

j=3.0*2;
print(j);

```


##### 2、字符串

```

字符串使用单引号或者双引号括起来   同时使用\转义特殊字符   



s='Yes,he doesn\'t';
print(s,type(s),len(s));

#如果不想让反斜杠发生转义  可以在字符串前面添加一个r,表示原始字符串
print('C:\some\name');
C:\some
ame


print(r'C:\some\name');
#C:\some\name  原样输出

#反斜杠可以作为续行符，表示下一行是上一行的延续
st='aaaaaaaaaaaaaaaaaaa\
bbbbbbbbbbbbbbbbbbbbb';
print(st);
#aaaaaaaaaaaaaaaaaaabbbbbbbbbbbbbbbbbbbbb

#可使用 '''.....'''跨越多行

str1='''
aaa
 bbb
   ccc
''';
print(str1);

aaa
 bbb
   ccc


#字符串可以使用 + 连接   * 运算符重复
print('str'+'ing','my'*3);
string mymymy

#两个紧邻的会自动拼接到一起
str3='hello' 'word';
print(str3);
hello word

#字符串的索引和切片
两种索引方式：
  1、从左往右 从0开始依次增加  
  2、从右往左  从-1开始 依次减少  
注意：Python中没有单独的字符类型，一个字符就是长度为一的字符串  
默认第一个索引为0，第二个索引默认为字符串可以被分切的长度  
切分字符串的时候有一个很有用的规律   word[:i]+word[i:]=word  

word='Python';
print(word[0],word[5]);
print(word[-6],word[-1]);  #两个输出结果一致

print(word[:2]);  # Py
print(word[2:]);  #thon
#一个过大索引将被字符串的大小取代   索引的上限值小于下限值  将返回一个空字符串
print(word[:100]);
print(word[20:]);  #20>0 空
print(word[2:1]);

#对字符串切片得到一串子串   格式 变量名[头下标：尾下标]   范围 前闭后开
print(word[0:3]);
print(word);#切片并不会对原字符进行改变

#需要注意的是 Python中的字符串是不可变的 向一个索引位置赋值，比如 word[0]='m' 会导致错误

#可以使用内置函数返回字符串的长度
print(len(word));

#字符串的判断
#可判断大小写  是否为字母  开头等

```

##### 3、 列表(List) 

```
# Python中使用最频繁的数据类型
a=['him',25,100,'her'];
print(a);
#同字符串  列表也支持切片和串联(+)操作  不同的是 列表中的元素是可以改变的
print(a[:3]);
a+=['zhangsan',100];
print(a);
a[0]='hh';
print(a);

#使用enumerate() 方法 同时获得索引位置和对应值
for i,v in enumerate(a):
    print(i,v)


#List的内置方法
#增 append()在末尾添加  extend() 合并进来一个列表
a.append("zhaoliu");
print(a);
a.insert(2,'lisi');
print(a);
b=[1,2,4];
a.extend(b);
print(a);

#改
a[0]='wangwu';
print(a);
#修改指定区间的列表值
a[2:5]=['B','C','D'];
print(a)
a[3:6]=[];#移除值
print(a)

#反转
a.reverse();
print(a);
#排序
#a.sort();  需都是字符串或者都是数字  否则无法进行比较
print(a);#按照ASCLL进行排序

#拷贝
a1=a.copy();
print(a1);
#查
print(a.index('wangwu'));#该元素所在位置的下标
print(a.count('wangwu'));#统计出现了几次
#同时遍历两个或者更多的序列  可以使用 zip（） 组合
questions=['names','quest','favorite color']
answers=['lancelot','the holy grail','blue']
for q,an in zip(questions,answers):
    print("What is your {0}? It is {1}.".format(q,an))

#遍历
for i in a:
    print(i);
#删
#清空列表

a.remove('wangwu');
print(a);
a.pop();
print("pop:",a); #删除尾部元素
a[:]=[];
print(a);
a.clear();
del a;
#print(a);此时  a已经是未定义的变量
#可使用列表的这些内置函数实现数据结构
#栈
stack=[3,4,5]
stack.append(6)
stack.append(7)
print(stack)

stack.pop()

print(stack)


#队列
from collections import deque
queue=deque(['Eric','John','Michal'])
queue.append('Terry')
queue.append('Graham')
print(queue)
queue.popleft()
print(queue)


#列表推导式
vec=[2,4,6]
ax=[3*x for x in vec]
print(ax)

bx=[[x,x**2] for x in vec]
print(bx)   #[[2, 4], [4, 16], [6, 36]]

#可使用 if 字句作为过滤器
cx=[3*x for x in vec if x>3]
print(cx)

vec1=[2,4,6]
vec2=[4,3,-9]
vec3=[x*y for x in vec1 for y in vec2]
print(vec3)

vec4=[vec1[i]*vec2[i] for i in range(len(vec1))]
print(vec4)
#列表推导式可以使用复杂表达式或嵌套函数
vec5=[str(round(355/113,i)) for i in range(1,6)]  #当你在文件中定义了一个str变量时  此处的str()就会报错
print(vec5)

#嵌套列表的解析
martrix=[
    [1,2,3,4],
    [5,6,7,8],
    [9,10,11,12]
]
ch=[[row[i] for row in martrix] for i in range(4)]
print(ch)

```




##### 4、 元组  

```
#与列表相似  只是 列表中的元素是不可改变的
c=(1990,2018,'python','c++');
print(c);
```

##### 5 、集合

```
#集合  无序不重复的的集  可使用{}  或者 set()来创建
student={'zhangsan','zhangsan','lisi','lisi','wangwu'};
print(student);  #{'wangwu', 'lisi', 'zhangsan'}  重复元素自动去掉
#创建一个空的集合
stu=set();
print(stu);
print('zhangsan' in student);  #判断元素是否在集合中

e=set('abracadabra');
print(e);  #{'a', 'c', 'b', 'r', 'd'}
f=set('alacazam');
print(f);


#差集
print(e-f);
#并集
print(e|f);
#交集
print(e&f);
# 两个集合不同时存在的元素
print(e^f);

#集合也支持推导式

```
##### 6、 字典  

```
 无序的 键值对集合  其关键字  也就是键  必须是不可变类型  并且同一个字典中  键不可相同  
 #key-value的数据类型
#语法
info={
	'stu1101':"zhangsan",
    'stu1102':'lisi',
    'stu1103':['wangwu',18]
}
#字典是无序的   因为它不需要下标

#查找
print(info['stu1101'])
#print(info['stu1104']);#不存在会报错
print(info.get("stu1101")) #安全的获取方法   元素不存在就会输出 none
print(info.get('stu1104')); #None
print('stu1103' in info) # 判断某个元素是否在列表中
print(info.get('stu1103'));

#修改
info['stu1102']='xiaolan';
print(info)

#修改不存在的key值   则是创建一条
info['stu1104']='xiaoming';
print(info);





#删除
del  info["stu1101"]
print(info);
info.pop("stu1102")
print(info)
info.popitem()#随机删除   别用
print(info);

print(info.items()); #把一个字典转换为列表



#字典的遍历
for i in info:
	print(i,info[i]);


for k,v in info.items():
	print(k,v)  #不要使用它   因为要把字典转换为列表  如果字典非常的大  光转换都得很长的时间

#也可使用dict()函数直接对键值对元祖列表中构建字典
mydict=dict([('saoe',4139),('guido',4127),('jack',4098)])
print(mydict)
#如果关键字只是简单的字符串  使用关键字参数指定键值对有时候更方便
mydict1=dict(space=4139,guido=4127,jack=4098)
print(mydict)


```


#### 二、if_else 分支
```
age=int(input("Age of the dog:"))
print()
if age<=0:
    print("the can hardly be true!")
elif age==1:
    print("about 14 human years")
elif age==2:
    print("about 22 human years")
elif age>2:
    human=22+(age-2)*5
    print("Human years:",human)

###
input("press Return")
```

#### 三、 循环

```
#while
n=100

sum=0
counter=1
while counter<=n:
    sum=sum+counter
    counter+=1
print("Sum 0f 1 until %d:%d"%(n,sum))

#for  与C语言不同的是 for循环也可以有 else块
languages=["c","C++","Perl","Python"]
for x in languages:
    print(x);

#break
edibles=["ham","spam","eggs","nuts"]
for food in edibles:
    if food=="spam":
        print("No more spam please!")
        break
    print("Great,delicious"+food)
else:
    print("I am so glad: No spam!")
print("Finally, I finished stuffing myself")

#range()函数  用于遍历数字序列
for i in range(5):
    print(i)
for i in range(5,9):
    print(i);
#range()函数也可指定步长
for i in range(0,10,3):
    print(i)


#负数
for i in range(-10,-100,-30):
    print(i)

#也可以借助 range()和len()函数以遍历一个序列的索引
a=['Mary','had','a','little','lamb']
for i in range(len(a)):
    print(i,a[i])

#也可以使用range()函数来构建一个列表
l=list(range(5))
print(l)


```

#### 四、函数

```
def hello():
    print("hello world！")

hello()


def area(width,height):
    return width*height
def print_welcome(name):
    print("Welcome",name)
print_welcome("Fred")
w=4
h=5
print("width=",w,"height=",h,"area=",area(w,h))

#可变参数列表
def arithmetic_mean(*args):
    sum=0;
    for x in args:
        sum+=x
    return sum

print(arithmetic_mean(45,32,89,78))
print(arithmetic_mean(45))
print(arithmetic_mean())

```

#### 五、导入模块
##### 1、初识模块

```
import sys

sys.py
import sys
不可使用 
当前目录下不可存在要引入模块的同名文件，否则就会被屏蔽掉
#查看系统的环境变量
print('命令行参数如下：')
for i in sys.argv:
    print(i)
print('/n/nThe PythonPATH is',sys.path,'/n')
#自定义模块  自定义模块可放在当前目录  或者 sys.path中的一个目录中  一般放在 site_packages 下
# '/home/nwpu/PycharmProjects/practice/venv/lib/python3.5/site-packages'
import fibo
fibo.fib(10)
fibo.fib2(10)
#导入模块并没有把定义在fibo中的含糊名称写在当前符号表里，只是把模块fibo的名字写在了那里
#如果打算经常使用一个函数，你可以把它赋给一个本地的名称
fib=fibo.fib
fib(100)
#可以使用内置函数以字符串格式返回模块内定义的所有名称
mydir=dir(fibo)
print(mydir)

```

######  2、 深入模块

__name__属性

```
test.py文件中代码如下：
print("I'm the first.")
print(__name__)
if __name__=="__main__":
     print("I'm the second.")

import_test.py中代码如下：
import test 

直接执行test.py结果如下
I'm the first.
__main__
I'm the second.

执行 import_test.py结果如下：
I'm the first.
test

```

<span style="color:red;">
可以得出结论：若一个Python文件为正在执行文件时
\__name__=="\__main__",否则它等于其文件名
</span>

#### 六、输入输出

```
#Python 中输入与输出

# 1、表达式  终端下采取交互式的方式输入的时候
#2、print()函数
#3、write()
#4、如果想要输出的形式更加多样 采用str.format()函数来格式化输出值
#5、若希望将输出的值转换为字符串，可以使用 repr()或者str()函数来实现

str1=str(1/7)
print(str1)

str2=repr(str1)  #0.14285714285714285
print(str2)  #'0.14285714285714285'

x=10*3.25
y=200*300
s='The value x is '+repr(x)+',and y is '+repr(y)+'...'
print(s)  #repr() 函数可以转义字符串中的特殊字符

for x in range(1,11):
    print('{0:2d}{1:3d}{2:4d}'.format(x,x*x,x*x*x)) #{}及里面的格式化字段都会被format（）中的参数置换掉

import math
print('The value of PI is approxiamtely {0:.3f}'.format(math.pi))#小数点后暴露三位
#zfill()方法会在左边填0
str3='12'.zfill(5)
print(str3)


#读和写文件
#open()函数将会返回一个file对象  file(filename,mode)
# f.read(size) 读取一定目的个数的数据  如果size的值不写或者为负值  则所有的数据都会被读取出来
#f.readline()函数将会读取一行  如果返回空字符串则说明已经读取到最后一行
f=open('/tmp/test','r')
print(f.read())
f.close()

fp=open('/tmp/test','r')
i=0;
while(fp.readline()!=''): #逐行读取出来
    print(str(i)+fp.readline())
    i=i+1

fp.close()

fp1=open('/tmp/test','r')
print(fp1.readlines())#所有的内容返回一个字典
fp1.close()

#也可采用迭代的方式进行读取
fp2=open('/tmp/test','r')
for line in fp2:
    print(line,end='')
fp2.close()

#可以使用write()方法将内容写入进去
fp3=open('/tmp/test','w')  # w会覆盖掉原有的内容  a是追加
fp3.write('Hello world!!!')

num=1000
strnum=str(num)
fp3.write(strnum) #如果想给文件中写入非字符串的东西  需现将其转换为字符串

#f.tell()返回对象所处的位置  它是从文件开头算起的字节数
print(fp3.tell())

#f.seek()
#fp3.seek(x,0) 从文件开头移动x个位置
#fp3.seek(x,1) 从当前位置往后移动x个位置
#fp3.seek(-x,2) 从文件结尾移动X个位置


#pickle模块  实现基本的数据序列和反序列化  它可以把 列表  字符串等保存到文件中  并且可以解析处理啊
import pickle
data1={'a':[1,2.0,3,4+6j],
       'b':('string',u'Unicode string'),
       'c':None}
selfref_list=[1,2,3]
selfref_list.append(selfref_list)
output=open('/tmp/data.pk1','wb')
pickle.dump(data1,output)
pickle.dump(selfref_list,output,-1)
output.close()

#对刚才保存的内容进行解析
pk1_file=open('data.pk1','rb')
data1=pickle.load('data.pk1','rb')
data1=pickle

```

#### 七、异常处理

```
#异常处理
while True:
    try:
        x=int(input("Please enter a number"))
        break
    except ValueError:
        print("Oops! That was no valid number. Try again")


#try except 还有一个可选的else 语句 可以把文件关闭等必要的操作放在else语句中去

import sys
for arg in sys.argv[1:]:
    try:
        f=open(arg,'r')
    except IOError:
        print("can on open",arg)
    else:
        print(arg,'has',len(f.readlines()),'lines')
        f.close()


#抛出异常
try:
    raise NameError('HiThere')
except NameError:
    print('An exception flew by!')
    raise


#自定义异常  异常应该继承自Exception类 或者直接继承  或者间接继承
class MyError(Exception):
    def _init_(self,value):
        self.value=value
    def __str__(self):
        return repr(self.value)

try:
    raise  MyError(2*2)
except MyError as e:
    print('My exception occured,value:',e.value)

#finnaly中的语句一定会被执行

```

#### 八、类

```
class people:
    #定义基本属性
    name=''
    age=0

    #定义私有属性，私有属性在类外部无法直接访问
    _weight=0
    #_init_() 类似与  类似于C++中的构造方法  可以有参数

    def _init_(self,n,a,w):
        self.name=n
        self.age=a
        self._weight=w
    #类方法必须有参数 self 并且必须为第一个参数
    def speak(self):
        print("%s is speaking: I am %d years old"%(self.name,self.age))

p=people('tom',10,30)
p.speak()

#类的继承--单继承
class student(people):
    grade=''
    def _init_(self,n,a,w,g):
        #调用父类的构造函数
        people._init_(self,n,a,w)
        self.grade=g
    #复写父类的方法
    def speak(self):
        print("%s is speaking: I am %d years old,and i am in grade %d"%(self.name,self.age,self.grade))

s=student('ken',20,60,3)
s.speak()

class speaker:
    topic=''
    name=''
    def _init_(self,n,t):
        self.name=n
        self.topic=t
    def speak(self):
        print("I am %s,I am a speaker! My topic is %s"%(self.name,self.topic))


class sample(speaker,student):
    a=''
    def _init_(self,n,a,w,g,t):
        student._init_(self,n,a,w,g)
        speaker._init_(self,n,t)
test=sample("Tim",25,80,4,"Python")
test.speak()
```

#### 九、常用标准模块

```
import os
pt=os.getcwd() #返回当前的工作目录
print(pt)
#os.chdir('/tmp')  #修改当前的工作目录

#os.system('mkdir tody')  #执行系统命令 mkdir  由于刚才切换工作目录是在 /tmp目录下  因此文件被创建在/tmp下
#建议使用 import os  而不是使用 from os import  这样 os.open()不会覆盖掉 open()

#文件通配符  glob
import glob
mylist=glob.glob('*.py')
print(mylist)

#命令行参数
import sys
print(sys.argv)

sys.stderr.write('Warning,log file not found starting a new one\n')

#字符串正则匹配
import  re
re.finditer(r'\bf[a-z]*','which foot or hand fell fastest')

re.sub(r'(\b[a-z]+) \1',r'\1','cat in the hat')

import math
mycos=math.cos(math.pi/4)
mylog=math.log(1024,2)
print(mycos)
print(mylog)

#random
import random
myrandom=random.choice(['apple','pear','banana'])
print(myrandom)  #apple  随机选取一个

#产生0-9之间的随机数
print("0-9之间的随机数",random.randint(0,9))

myrandom1=random.sample(range(100),10)
print(myrandom1)
myrandom2=random.random()
print(myrandom2)

#访问互联网  用到再说

#日期和时间 datetime 模块为日期和时间处理同时提供了简单和复杂的方法
from datetime import  date
now=date.today()
print(now)
#使用日历模块显示日期
import calendar
yy=int(input("输入月份："))
mm=int(input("输入年份:"))
print(calendar.month(yy,mm))
#也可计算每个月的天数
monthRange=calendar.monthrange(2013,6)
print(monthRange)  #(5,30)  意思是6月份有30天


#数据压缩
import zlib
s=b'witch witch has which witches wrist watch'
print(len(s))
t=zlib.compress(s)
print(len(t))


```

#### 十、常用内置方法

```
#直接使用max()函数即可输出最大和最小值
print(max(1,2))


#进制转换
dec=int(input("输入数字："))
print("十进制为",dec,":")
print("转换为二进制为：",bin(dec))
print("转换为八进制为：",oct(dec))
print("转换为十六进制为：",hex(dec))


#ASCII与字符之间的转换
c=input("请输入一个字符：")
a=int(input("请输入一个ASCII码："))
print(c+"的ASCLL码为",ord(c))
print(a,"对应的字符为",chr(a))
```
