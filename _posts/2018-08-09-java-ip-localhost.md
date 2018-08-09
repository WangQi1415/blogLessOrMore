---
layout: post
title:  "Java获取本机IP"
date:   2018-08-09 14:39:01 +0800
categories: Java学习
tag: java
---

* content
{:toc}


#### 获取本地IP地址的方法如下：  
```
public class TestIp {
	public static void main(String[] args) throws UnknownHostException {
		 InetAddress addr = InetAddress.getLocalHost();
	      System.out.println("Local HostAddress: "+addr.getHostAddress());
	      String hostname = addr.getHostName();
	      System.out.println("Local host name: "+hostname);
	}
}
```

