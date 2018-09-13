---
layout: post
title:  "Python中的数据存储"
date:   2018-09-09 11:40:01 +0800
categories: Python
tag: Python
---

* content
{:toc}

### 学习目标
本次的学习目标是编写一个Python程序来管理员工信息，教程中的内容参考自书籍 Python programming 第四版，它在第一章采用这种“提出解决方案，然后再推翻，再提出...，直到找到合理的解决方案为止”的学习方法，令我感受颇深，所以很是推荐这本书，希望对大家也会有所帮助。
###  第一步：表示记录
#### 使用List

```
'''

bob=['Bob Smith',42,30000,'software']
sue=['Sue Jones',45,40000,'hardware']
print(bob[0],sue[2])
print(bob[0].split()[-1]) #输出bob的姓

'''
split()：拆分字符串。通过指定分隔符对字符串进行切片，并返回分割后的字符串列表（list）
语法：str.split(str="",num=string.count(str))[n]
参数说明：
str：表示为分隔符，默认为空格，但是不能为空('')。若字符串中没有分隔符，则把整个字符串作为列表的一个元素
num：表示分割次数。如果存在参数num，则仅分隔成 num+1 个子字符串，并且每一个子字符串可以赋给新的变量
[n]：   表示选取第n个分片

注意：当使用空格作为分隔符时，对于中间为空的项会自动忽略
'''

people=[bob,sue]
for person in people:
    print(person)
print(people[1][0])
for person in people:
    print(person[2])
#收集薪酬信息
#方法一  列表解析
pays1=[person[2] for person in people]
print(pays1)
#方法二 map()
'''
语法：map(function, iterable, ...)
参数详情 
function:将集合的每个元素当做参数传递给该函数
iterable: 可迭代的集合，列表，元组等

返回值： map()的返回值是一个map()对象  可以使用list(),set()函数来创建列表或者集合
'''
pays2=map((lambda x:x[2]),people)
print(pays2)
print(list(pays2))

#像人员数据库中添加数据
people.append(['Tom',50,0,None])
print(len(people))
print(people[-1][0])
```
由上可见，使用列表是可以实现数据存储的功能，但是这样就存在一个问题，我们必须记住每个位置的含义，比如第一个位置是姓名，第二个位置是薪酬。这样就非常麻烦。想要解决这个问题，最好的方法是把字段名和字段值关联起来，该如何实现呢？ 我们可以先试着使用Python的内置。
```
NAME,AGE,PAY=range(3)
print(bob[NAME])
print(PAY,bob[PAY])
```
这样貌似是解决了问题，当我们改变记录的结构时，比如 再加一个性别，性别如果放在第二个位置，那就都需要改变，维护的成本太高了，明显是不可行的，我们再试着使用元祖的列表来解决这个问题。

```
bob=[['name','Bob Smith'],['age',42],['pay',10000]]
sue=[['name','Sue Jones'],['age',45],['pay',20000]]
people=[bob,sue]
for person in people:
    print(person[0][1],person[2][1])

```
这样并没有解决问题  我们仍然需要通过位置来获取信息,实际上只是仅仅增加了一层额外的位置索引。为了准确的获取我们想要的信息，我们或许可以再通过遍历所有人员信息，进行判断得到自己想要的。  
比如如果想要或得所有的姓名，我们可以在循环中进行判断：  
```
for person in people:
    for (name,value) in person:
        if name=='name':
            print(value)
```
为了增加代码的可用性，我们可以将这个循环封装成一个方法  
```
#为了增加代码的可用性  我们可以将其封装成一个函数
def field(record,label):
    for(fname,fvalue) in record:
        if fname==label:
            return fvalue
print(field(bob,'name'))
print(field(sue,'pay'))

#打印所有的年龄
for rec in people:
    print(field(rec,'age'))
```
如果已经对Python有了一定的了解，你会知道上述的实现都可以内置的字典对象实现
#### 使用字典
存储数据
```
bob={'name':'Bob Smith','age':42,'pay':30000,'job':'dev'}
sue={'name':'Sue Jones','age':45,'pay':40000,'job':'hdw'}
print(bob['name'],sue['pay'])
print(bob['name'].split()[-1])
sue['pay']*=1.10
print(sue['pay'])

```
进行遍历
```
people=[bob,sue]
for person in people:
    print(person['name'],person['pay'],sep=',')
#使用迭代工具进行遍历
names=[person['name'] for person in people]
print(names)
names1=list(map(lambda x:x['name'],people))
print(names1)

#汇总薪水
sum=sum(person['pay'] for person in people)
print(sum)

```
比较给力的是 使用列表竟然能够实现SQL查询语句相同的效果
```
print([rec['name'] for rec in people if rec['age']>=45])
print([rec['age']**2 if rec['age']>=45 else rec['age'] for rec in people])
```
进一步改进，如果想直接获得bob的薪资该怎么办呢？  使用字典的字典（嵌套）
```
db={}
db['bob']=bob
db['sue']=sue
print(db['bob']['pay'])

#逐条的访问数据库
for key in db:
    print(key,'=>',db[key]['name'])
#新增一条记录
db['Tom']=dict(name='Tom',age=50,job=None,pay=0)
print(db['Tom'])
```
<p style="color:red">以上的存储我们便完成了Python的数据存储工作，但是新的问题就来了，当我们退出Python之后这些数据就会烟消云散，因此我们接下来需要做的就是持久化操作
</p> 
### 第二步：执行持久化存储
若需要人员信息持久化，我们需要将其保存到某种类型的文件中，所以我们先回顾复习下文件的基本操作。
#### 文件的基本操作 
#####  打开
open(para1,para2,para3)  
&nbsp;&nbsp;  para1: 表示打开文件的名称  
&nbsp; &nbsp; para2：表示文件的打开方式  
 &nbsp; &nbsp; &nbsp; &nbsp;  w 以写的方式打开  
  &nbsp; &nbsp; &nbsp; &nbsp;  r 以 读的方式打开  
   &nbsp; &nbsp; &nbsp; &nbsp;  a 表示在文件尾部追加内容而打开文件  
   &nbsp; &nbsp; &nbsp; &nbsp;  b 可以进行二进制数据处理  
   &nbsp; &nbsp; &nbsp; &nbsp;  + 表示同时为输入和输出打开文件  
   &nbsp; &nbsp; para3:控制输出缓存  传入0 表示 输出无缓存  
```
dbfilename='people-file'
dbfile=open(dbfilebname,'+')
```

##### 写入
dbfile.write(asString) 写入字节字符串到文件  
dbfile.write(aList) 把列表中的所有字符串写入文件  
print(key,file=dbfile) 等价于 dbfile.write(key+'\n')  
##### 读取
dbfile.read() 整个文件读取到一个字符串  
dbfile.readline() 读取下一行（包括行末标识符）到一个一个字符串  
dbfile.readlines() 读取整个文件到一个字符串列表  
使用文件迭代器进行读取（推荐）  
```
for line in open('data'):
	print(line,end=' ')
```

##### 关闭
dbfile.close() 手动关闭文件
dbfile.flush() 把输出缓冲区刷新到硬盘，但是不关闭文件

 注意！！！  
        从文件读取的数据回到脚本时是一个字符串。所以字符串如果不是你所需要的，就得将其转换为其他类型的Python对象。    
        同样与print语句不同的是，当你把数据写入文件时，Python 不会自动把对象转换为字符串--你必须传递一个已格式化的字符串

#### 保存人员数据到文件

```
dbfilename='people-file'
ENDDB='enddb.'
ENDREC='endrec.'
RECSEP='=>'
def storeDbbase(db,dfilename=dbfilename):
    dbfile=open(dbfilename,'w')
    for key in db:
        print(key,file=dbfile)#写入到文件
        for (name,value) in db[key].items():
            print(name+RECSEP+repr(value),file=dbfile)#repr返回对象的字符串形式，此处为什么要这样设置呢？
        '''
        注意！！！
        从文件读取的数据回到脚本时是一个字符串。所以字符串如果不是你所需要的，就得将其转换为其他类型的Python对象。
        同样与print语句不同的是，当你把数据写入文件时，Python 不会自动把对象转换为字符串--你必须传递一个已格式化的字符串
        '''
        print(ENDREC,file=dbfile)
    print(ENDDB,file=dbfile)
    dbfile.close()

def loadDbase(dbfile=dbfilename):
    dbfile=open(dbfilename)
    import sys
    sys.stdin=dbfile  #将input() 的输入由终端转为 dbfile,即由文件输入
    db={}
    key=input()
    while key!=ENDDB:
        rec={}
        field=input()
        while field != ENDREC:
            name,value=field.split(RECSEP) #进行分割
            rec[name]=eval(value)  #eval()函数十分强大，官方demo解释为：将字符串str当成有效的表达式来求值并返回计算结果。
            field=input()
        db[key]=rec
        key=input()
    return db

if __name__=='__main__':
    storeDbbase(db)
    mydb=loadDbase()
    print(mydb)
```
<p style="color:red">以上便已经实现了数据的持久化，但是存在以下几个问题：</p>
1. 即便是为了获取一条记录也需要从文件中读取整个数据库，而且每一次更新后也需要把所有数据重新写入文件。
2. 采用以上文本文件的方式鉴定写入文件的数据分隔符不会出现在存储的数据中。如果分隔符=>恰好出现在数据中，这种结构就会失败。

<b>因此，我们需要一种通用的工具，可以直接把Python中的数据转化为可以保存在文件中的格式，这个工具就是 pickle文件</b>

### 使用Pickle文件
何为pickle?
任意Python对象与字节串之间的序列化

Pickle模块是如何工作的？  
pickle模块将内存中的Python对象转化为序列化的字节流，这是一种可以写入任何一种类似文件对象的字符串。  
pickle模块也可以根据序列化的字节流重新构建原来内存中的对象（转换成原来那个对象）  
 
 因此，使用pickle使得我们只需要一个步骤就可以存储和获取Python对象，不用手动的存储和解析对象   
 
 使用Pickle存储人员信息与解析:  
 
```
from save import db
import pickle
dbfile=open('people-pickle','wb')
pickle.dump(db,dbfile)
dbfile.close()

dbfile=open('people-pickle','rb')
db=pickle.load(dbfile)
for key in db:
    print(key,"=>\n",db[key])
print(db['sue']['name'])
```
 
<b style="color:red">注意！</b> 
1. 当记录在内存中被修改后是将整个数据库重新写入文件，所以pickle在这方面是与手动的格式化文件的方式是一样的。当处理非常大的数据库时，会变慢。接下来会讨论这个问题。
2. 在Python3中 所有协议都是使用bytes对象来表示pickle数据，因此，需要使用位二进制模式来打开所有的pickle文件

### 每条记录使用一个pickle文件
前面我们提到，当一条记录发生改变时，我们需要将所有的数据都重新保存到数据库，这会使得数据库变慢，那如果我们将每一套数据存放到一个文件中，当数据改变时，更新相应的文件而不是整个数据库是不是会节省很多时间呢？
```
from save import bob,sue,tom
import  pickle
for(key,record) in [('bob',bob),('tom',tom),('sue',sue)]:
    recfile=open(key+'.pkl','wb')
    pickle.dump(record,recfile)
    recfile.close()


#读取所有的人员信息
import  pickle,glob
for filename in glob.glob('*.pkl'):
    recfile=open(filename,'rb')
    record=pickle.load(recfile)
    print(filename,"=>\n",record)

#若想获得某个人员信息，直接读取其文件
suefile=open('sue.pkl','rb')
sue=pickle.load(suefile)
print(sue['name'])
suefile.close()
sue['pay']*=1.5
suefile=open('sue.pkl','wb')
pickle.dump(sue,suefile)
suefile.close()

```
### 使用shelves
上述解决了文件存储时的效率问题，但是当你的文件很多的时候，管理起来很不方便，因此Python提供了一种更高层次的东西：Shelves  
Shelves的工作原理：  
shelves 自动将对象pickle进和pickle出键访问文件系统。它像必须打开着的字典，在程序退出时进行持久化。因为它使用键访问被存储记录，所以不需要手动为每条记录管理一个普通文件，shelves自动分隔存储记录，并且只获得和更新被访问和修改的记录，通过这种方式，shelves使用起来很像一堆只存储一条记录的pickle文件，但是更容易编写代码  

存储到shelves以永久保存
```
from  save import bob,sue
import shelve
db=shelve.open('people-shelve')
db['bob']=bob
db['sue']=sue
db.close()
#该脚本在当前目录下以people-shelve为前缀创建一个或者多个文件，在Win
```
重新打开并读取
```
db=shelve.open('people-shelve')
for key in db:
    print(key,'=>\n',db[key])
print(db['sue']['name'])
db.close()
```

<p style="color:red">上述方式实现了数据的存储，但是如果我想给员工加薪%10，管理者加薪%20，该如何实习呢？需要为每个操作写相应的代码，这会使得造成代码冗余，接下来就让我们走进OOP，来改善这种情况！</p>



