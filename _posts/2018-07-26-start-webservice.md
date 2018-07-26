---
layout: post
title:  "初识Web Service"
date:   2018-07-26 14:37:01 +0800
categories: 教程
tag: Web Service
---

* content
{:toc}


#### 1、何为 Web Service?
（1）它使得本地应用软件所提供的服务可以通过网络在远程得到使用，它们之间的消息传递是通过XML文件进行的。因此，Web Service不受操作系统和编程语言的限制，例如Ubuntu可以与Windows进行通信，Java可以与Python 进行通信。
（2）Web Service 基于像TCP/IP、HTTP、Java、HTML和XML等这些开放标准构建的，因此可以构建本地的、分布式的或者基于web的应用。
#### 2、WebService的组件
Web Service 的基础是XML+HTTP,W所有的Web Service 都会用到以下组件:  
（1）SOAP(Simple Object Access Protocol)[SOAP1.2](http://www.w3.org/TR/2003/REC-soap12-part0-20030624/)  
简单对象访问协议，用来描述传递信息的格式，它是基于XML的协议。主要有以下优点：

- 它是一种通信协议，可以在应用之间进行通信
- 平台独立性
- 语言独立性
- 简单和可扩展性
- 允许通过防火墙
- 遵循W3C标准

（2）UDDI(Universal Description,Discovery and Integration）  
用来管理，分发，查询 WebService  
（3）WSDL（Web Services Description Languange）[WSDL](https://www.w3.org/TR/2007/REC-wsdl20-primer-20070626/)  
用来描述如何访问具体的接口  


#### 3、应用实例

一个简单的账户管理和订单处理系统，会计人员用客户端（JSP或者其它）来创建新的账户，并且键入新的客户订单  
订单处理系统使用Java实现在其它电脑上并且与数据库进行交互  
我们采用以下步骤进行构建：  
1、客户端程序将账户注册信息捆绑到SOAP消息中  
2、将SOAP消息作为HTTP POST请求的消息体发送到 WebService端  
3、WebService端解包SOAP请求 并且将其转换为应用可以理解的命令  
4、WebService端处理请求 并且将响应信息放入另一个SOAP中响应给用户  
5、用户得到相应的数据，并且解包呈现出来  
#### 4、为什么使用WebService
1、可以暴露已有的方法到网络上  
2、互通性，允许多个应用相互之间进行通信，不限于系统和语言  
3、采用标准化协议  
4、相互通讯消耗低  
5、基于XML，使得跨系统，跨语言通信不受限制  
6、低耦合  
7、粗粒度  
8、能够同步或异步  
9、支持远程过程调用  
10、支持文档交换，表现在 XML不仅可以存放数据，也可以存放长文本  

#### 5、快速开始

(1) 环境配置  
Eclipse+Tomcat+Axis2([axis2](http://axis.apache.org/axis2/java/core/download.cgi) )  
Axis2选择 Binary Distribution  
Eclips中Axis2配置   
windows->prefrence->Axis2 Preferences->Browse(D:/axis2-1.7.8 自己解压的位置)->Apply  
（2） 创建项目  
    a. 创建java项目， Javaproject 或者 Dynamic项目均可。  
    b. 右键项目 new Web Service，配置Tomcat，Axis等，如图：  
    <img src="{{ '/styles/images/2018-7-26/2018-07-26_113237.jpg' | prepend: site.baseurl }}" alt="deploy" width="500" />  

（3）  服务器端实现  

```
package cn.nwpu.service;

import javax.jws.WebMethod;
import javax.jws.WebService;

@WebService
public interface Caculate {
	@WebMethod
	public int add(int a,int b);
	@WebMethod
	public int curt(int a,int b);	
}


package cn.nwpu.service;

import javax.jws.WebService;

@WebService
public class CaculateImpl implements Caculate {
	public int add(int a,int b) {
		return a+b;
	}
	public int curt(int a,int b) {
		return (a>b)?a-b:b-a;
	}
}

```
（4）  服务端点注册  

```
package cn.nwpu.server;

import javax.xml.ws.Endpoint;

import cn.nwpu.service.Caculate;
import cn.nwpu.service.CaculateImpl;

public class Server {
	 public static void main(String[] args) {
		Caculate ca=new CaculateImpl();
		String address="http://1X.2X.1XX.1XX:8080/Caculate";
		Endpoint.publish(address, ca);
	}
}

```
（5） 客户端生成  
新建项目->右键SRc->Show in Terminal  
然后在终端输入 wsimport -keep http://1X.2X.1XX.1XX:8080/Caculate?wsdl  
刷新项目 即可看见自动生成的包  
如图:  
  <img src="{{ '/styles/images/2018-7-26/1.jpg' | prepend: site.baseurl }}" alt="deploy" width="300" />  
 
 在Test.java中实现主函数进行测试  

 ```
package cn.nwpu.service;

public class Test {
	public static void main(String[] args) 
	{
		CaculateImplService cis=new CaculateImplService();
		CaculateImpl ci=cis.getCaculateImplPort();
		System.out.println(ci.add(1, 2));
		System.out.println(ci.curt(3, 2));
	}
}
 ```
 参考文献  
 [https://www.tutorialspoint.com/webservices/web_services_summary.htm](https://www.tutorialspoint.com/webservices/web_services_summary.htm) 
[ https://blog.csdn.net/xw13106209/article/details/7049614]( https://blog.csdn.net/xw13106209/article/details/7049614) 
 