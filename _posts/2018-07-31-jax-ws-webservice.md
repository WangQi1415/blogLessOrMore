---
layout: post
title:  "使用Maven部署基于JAX-WS的 WebService"
date:   2018-07-31 14:37:01 +0800
categories: 教程
tag: Web Service
---

* content
{:toc}


#### 1、JAX-WS概述

JAX-WS2.0 的全称为 Java API for XML-Based Webservices (JAX-WS) 2.0。JAX-WS 2.0 是对 JAX-RPC 1.0 规范的扩展，是 JAX-RPC 1.1 的后续版本， JAX-RPC 2.0 标准发布不久后便被重新命名为 JAX-WS 2.0。 JAX-WS 2.0 是面向 Java 5 的开发 Web services 的最新编程标准，它提供了新的编程模型和对以往的 JAX-RPC 方式的 Web services 进行了增强。 JAX-WS2.0 (JSR 224)是Sun新的web services协议栈，是一个完全基于标准的实现。在binding层，使用的是the Java Architecture for XMLBinding (JAXB, JSR 222)，在parsing层，使用的是the Streaming API for XML (StAX, JSR 173)，同时它还完全支持schema规范。

#### 2、使用maven构建WebService项目
项目架构:  
  <img src="{{ '/styles/images/2018-7-31/1.jpg' | prepend: site.baseurl }}" alt="deploy" width="300" />  
 

1、 创建服务接口  


 ```
 
package cn.nwpu.service;

import javax.jws.WebMethod;
import javax.jws.WebService;
import javax.jws.soap.SOAPBinding;
import javax.jws.soap.SOAPBinding.Style;

@WebService
@SOAPBinding(style = Style.RPC)
public interface HelloWorldService {
	 @WebMethod
     public String sayHello();
}


 ```
 2、  服务接口实现  
 
 ```
 
 package cn.nwpu.serviceimpl;

import javax.jws.WebService;

import cn.nwpu.service.HelloWorldService;

@WebService(endpointInterface = "cn.nwpu.service.HelloWorldService")
public class HelloWorldServiceImpl implements HelloWorldService {

	public String sayHello() {
		// TODO Auto-generated method stub
		return "Hello from Programmer Gate ..";
	}

}

 
 ```
 3、  创建 sun-jaxws.xml  
 
 ```
 
 <?xml version="1.0" encoding="UTF-8"?>
<endpoints
  xmlns="http://java.sun.com/xml/ns/jax-ws/ri/runtime"
  version="2.0">
  <endpoint
      name="HelloWorldService"
      implementation="cn.nwpu.serviceimpl.HelloWorldServiceImpl"
      url-pattern="/hello"/>
</endpoints>


 ```
 
 4、  创建web.xml  
 
 ```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="MavenJax" version="3.1">
  <display-name>MavenJax</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
    <listener>
        <listener-class>
                com.sun.xml.ws.transport.http.servlet.WSServletContextListener
        </listener-class>
    </listener>
    <servlet>
        <servlet-name>hello</servlet-name>
        <servlet-class>
            com.sun.xml.ws.transport.http.servlet.WSServlet
        </servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
    <session-config>
        <session-timeout>120</session-timeout>
    </session-config>
</web-app>

 ```
 
 
 5、  在pom.xml文件中导入依赖  
 
 ```
 <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>cn.nwpu</groupId>
  <artifactId>MavenJax</artifactId>
  <packaging>war</packaging>
  <version>0.0.1-SNAPSHOT</version>
  
  <dependencies>
	<dependency>
		<groupId>com.sun.xml.ws</groupId>
		<artifactId>jaxws-rt</artifactId>
		<version>2.2.10</version>
	</dependency>
  	<dependency>
	    <groupId>javax.servlet</groupId>
	    <artifactId>javax.servlet-api</artifactId>
	    <version>3.1.0</version>
	    <scope>provided</scope>
	</dependency>
	<dependency> 
	   <groupId>javax.servlet.jsp</groupId> 
	   <artifactId>jsp-api</artifactId> 
	   <version>2.1</version> 
	   <scope>provided</scope>
	</dependency>
  </dependencies>

 ```
 
 6、 run on server   
 即可通过 localhost:8080/as?wsdl 访问

 