---
layout: post
title:  "简单的搭建一个FTP服务器"
date:   2018-07-10 14:37:01 +0800
categories: practice
tag: ftp
---

* content
{:toc}


想让自己的台式机跟自己的笔记本可以进行文件传输  虽然QQ什么挺好的吧  但是Ubuntu上下载个QQ感觉怪怪的，因此动手搭建自己的Ubuntu ftp服务器并记录下自己搭建的过程  
#### 1. 安装ftp
1.安装：sudo apt install vsftpd  
2.配置 vsftpd.conf  
<pre><code>	#禁止匿名访问
	anonymous_enable=NO
	#接受本地用户
	local_enable=YES
	#允许上传
	write_enable=YES
	#用户只能访问限制的目录
	chroot_local_user=YES
	#设置固定目录，在结尾添加。如果不添加这一行，各用户对应自己的目录，当然这个文件夹自己建
	local_root=/home/ftp</code></pre>
3.添加ftp用户  
<pre><code>sudo useradd -d /home/ftp -M ftpuser
sudo passwd ftpuser</code></pre>


4.修改文件夹权限  
`sudo chmod 777 /home/ftp`

5.修改 pam.d/vsftpd  
<pre><code>sudo gedit /etc/pam.d/vsftpd
注释掉auth required pam_shells.so
 #auth required pam_shells.so</code></pre>


6.重启 vsftpd  
`sudo service vsftpd restart`

此时就可以使用刚才建的这个用户登录ftp服务器了 看到的是 local_root 设置的/home/ftp  并且限制在该目录  
可以通过以下几种方法进行访问
1. 在浏览器中输入 ftp://XXXX.XXXX.XXXX.XXX进行访问
2. 也可以在dos命令下访问
3. 使用flashFXP进行文件的提交

#### 2. 使用dos命令进行提交
<pre><code>C:\Users\wq>ftp ~~10.xx.xx.xx
连接到 1x.xx.xx.xx
220 (vsFTPd 3.0.3)
用户(10.13.34.11:(none)): xx
331 Please specify the password.
密码:
230 Login successful.
ftp> dir
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-------    1 1003     1003      1733488 Apr 10 13:47 2.zip
-rw-------    1 1003     1003          230 Apr 09  2017 const修饰的成员函数剖解.
txt
drwxr-xr-x    2 0        0            4096 Jul 05 20:17 data
226 Directory send OK.
ftp: 收到 210 字节，用时 0.00秒 70.00千字节/秒。
ftp> send d:/ftp/a.txt
200 PORT command successful. Consider using PASV.
150 Ok to send data.
226 Transfer complete.
ftp: 发送 4 字节，用时 0.04秒 0.10千字节/秒。</code></pre>

#### 3. Ubuntu终端中文乱码的问题
这是因为Windows默认的编码格式是GBK格式 而Ubuntu默认的编码格式是UTF-8，因此导致乱码。  
解决方案：  
	在windows下 使用notepad++ 将文件的编码改成UTF-8再进行发送，
	需编码格式一直才不会产生乱码的现象