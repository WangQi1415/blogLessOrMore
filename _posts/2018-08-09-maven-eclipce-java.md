---
layout: post
title:  "Maven项目资源文件位置及其读取方法"
date:   2018-08-09 14:37:01 +0800
categories: Java学习
tag: maven
---

* content
{:toc}



###Maven项目资源文件位置及其读取方法
将文件放在 src/main/resource/文件下:  
<img src="{{ '/styles/images/2018-08-09/maven.JPG' | prepend: site.baseurl }}" alt="maven" width="300" />  
当项目编译运行过之后，文件会存放在 D:\webservice\AsMaven\target\classes\file下

可使用以下方法访问:  
```
System.out.println("classpath路径： "+FileRead.class.getClassLoader().getResource("").getPath());
System.out.println("文件所在路径： "+FileRead.class.getClassLoader().getResource("file/rusult.xml").getPath());
System.out.println("当前类路径： "+FileRead.class.getResource("").getPath());
```
输出结果：  
```
classpath路径：D:/webservice/AsMaven/target/classes/
文件所在路径：D:/webservice/AsMaven/target/classes/D:/webservice/AsMaven/target/classes/file/rusult.xml
当前类路径：   D:/webservice/AsMaven/target/classes/cn/nwpu/fileread/
```


