---
layout: post
title:  "JDK环境变量配置之小结——～/.bashrc、/etc/profile区别"
date:   2018-08-08 14:37:01 +0800
categories: 随笔杂谈
tag: Linux
---

* content
{:toc}



###JDK环境变量配置之小结——～/.bashrc、/etc/profile区别
####问题描述：
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Ubuntu16.04系统，原来配置了32位的JDK（初做练习的时候安装），并且在~/.bashrc文件中配置如下

```
export JAVA_HOME=/java/jdk1.8.0_171
export CLASSPATH=${JAVA_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
```
查看JDK版本 java -version

```
java version "1.8.0_171"
Java(TM) SE Runtime Environment (build 1.8.0_171-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.171-b11, mixed mode)

```

因需要，昨天在电脑上配置64位的JDK1.8，并且在 /etc/profile中配置如下

```
export JAVA_HOME=/java/java1.8
export JRE_HOME=/java/java1.8/jre
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH 
export PATH=$JAVA_HOME/bin:$PATH
```
配置完之后 source /etc/profile
此时查看JDK版本

```
java version "1.8.0_181"
Java(TM) SE Runtime Environment (build 1.8.0_181-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.181-b13, mixed mode)
nwpu@nwpu-Lenovo-Product:/etc$ source ~/.bashrc
```
重新打开个终端，并且查看JDK版本

```
java version "1.8.0_171"
Java(TM) SE Runtime Environment (build 1.8.0_171-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.171-b11, mixed mode)
```
bashrc与profile的区别 
要搞清bashrc与profile的区别，首先要弄明白什么是交互式shell和非交互式shell，什么是login shell 和non-login shell。   
交互式模式就是shell等待你的输入，并且执行你提交的命令。这种模式被称作交互式是因为shell与用户进行交互。这种模式也是大多数用户非常熟悉的：登录、执行一些命令、签退。当你签退后，shell也终止了。 shell也可以运行在另外一种模式：非交互式模式。在这种模式下，shell不与你进行交互，而是读取存放在文件中的命令,并且执行它们。当它读到文件的结尾，shell也就终止了。   
bashrc与profile都用于保存用户的环境信息，bashrc用于交互式non-loginshell，而profile用于交互式login shell。系统中存在许多bashrc和profile文件，下面逐一介绍：   
/etc/pro此文件为系统的每个用户设置环境信息,当第一个用户登录时,该文件被执行. 
并从/etc/profile.d目录的配置文件中搜集shell的设置.   
/etc/bashrc:为每一个运行bash shell的用户执行此文件.当bash shell被打开时,该文件被读取。有些linux版本中的/etc目录下已经没有了bashrc文件。 
~/. pro每个用户都可使用该文件输入专用于自己使用的shell信息,当用户登录时,该 文件仅仅执行一次!默认情况下,它设置一些环境变量,然后执行用户的.bashrc文件. 
~/.bashrc:该文件包含专用于某个用户的bash shell的bash信息,当该用户登录时以及每次打开新的shell时,该文件被读取.   
另外,/etc/profile中设定的变量(全局)的可以作用于任何用户,而~/.bashrc等中设定的变量(局部)只能继承/etc/profile中的变量,他们是"父子"关系.   

某网友总结如下：
etc/profile，/etc/bashrc 是系统全局环境变量设定   
~/.profile，~/.bashrc用户家目录下的私有环境变量设定   
当登入系统时候获得一个shell进程时，其读取环境设定档有三步   
1首先读入的是全局环境变量设定档/etc/profile，然后根据其内容读取额外的设定的文档，如 /etc/profile.d和/etc/inputrc   
2然后根据不同使用者帐号，去其家目录读取~/.bash_profile，如果这读取不了就读取~/.bash_login，这个也读取不了才会读取 ~/.profile，这三个文档设定基本上是一样的，读取有优先关系   
3然后在根据用户帐号读取~/.bashrc 
至于~/.profile与~/.bashrc的不区别 
都具有个性化定制功能   
~/.profile可以设定本用户专有的路径，环境变量，等，它只能登入的时候执行一次   
~/.bashrc也是某用户专有设定文档，可以设定路径，命令别名，每次shell script的执行都会使用它一次   

参考博客：[https://www.cnblogs.com/cwp-bg/p/8257843.html](https://www.cnblogs.com/cwp-bg/p/8257843.html) 



