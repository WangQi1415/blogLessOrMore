---
layout: post
title:  "Windows下Maven的配置"
date:   2018-08-09 18:37:01 +0800
categories: Java学习
tag: maven
---

* content
{:toc}



#### 1、下载：  
官网：[https://maven.apache.org/download.cgi](https://maven.apache.org/download.cgi)   
网盘链接：[https://pan.baidu.com/s/1EfOfU5IhewEGaxE0RnNHIQ](https://pan.baidu.com/s/1EfOfU5IhewEGaxE0RnNHIQ)  密码：sfm3 

#### 2、配置环境变量

系统环境变量里，添加MAVEN_HOME(或M2_HOME)，其值为D:\maven，然后PATH环境变量最后附加上";%MAVEN_HOME%\bin"  

检测方法：  

a) 重新进入命令行(DOS窗口)模式，输入 echo %MAVEN_HOME% 如果能显示 D:\maven 说明环境变量起作用了  

b) 输入 mvn -version，正常情况下会显示maven及jdk的版本号，（前提：jdk环境必须先安装好，否则后面无法正常编译项目）。  

#### 3、更改setting.xml文件 
位于 D:\maven\conf（复制网盘中的setting.xml文件替换即可）  
主要更改了以下两个位置:  

```
  <mirror>
      <id>alimaven</id>
      <name>aliyun maven</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
      <mirrorOf>central</mirrorOf>        
   </mirror>


<localRepository>D:/maven_rep</localRepository>
```
#### 4、在eclipse中配置加载setting.xml文件
preferences -> User Settings ->Browse User Setting->(setting.xml文件所在的位置)->apply

