---
layout: post
title:  "Ubuntu配置shadowsocks"
date:   2018-10-25 14:30:01 +0800
categories: Ubuntu
tag: VPN
---

* content
{:toc}


## 一、Ubuntu14.04安装shadowsocks
参考博客：  
[https://www.sundabao.com/ubuntu%E4%BD%BF%E7%94%A8shadowsocks/](https://www.sundabao.com/ubuntu%E4%BD%BF%E7%94%A8shadowsocks/)   
[https://www.switchyomega.com/settings/](https://www.switchyomega.com/settings/)   
### 1、安装shadowsocks
```
sudo apt-get update
sudo apt-get install python-pip
sudo apt-get install python-setuptools m2crypto
pip install shadowsocks
```
### 2、配置shadowsocks
在终端输入 sslocal --help 可查看帮助
```
Proxy options:
  -c CONFIG              path to config file
  -s SERVER_ADDR         server address
  -p SERVER_PORT         server port, default: 8388
  -b LOCAL_ADDR          local binding address, default: 127.0.0.1
  -l LOCAL_PORT          local port, default: 1080
  -k PASSWORD            password
  -m METHOD              encryption method, default: aes-256-cfb
  -t TIMEOUT             timeout in seconds, default: 300
  --fast-open            use TCP_FASTOPEN, requires Linux 3.7+

General options:
  -h, --help             show this help message and exit
  -d start/stop/restart  daemon mode
  --pid-file PID_FILE    pid file for daemon mode
  --log-file LOG_FILE    log file for daemon mode
  --user USER            username to run as
  -v, -vv                verbose mode
  -q, -qq                quiet mode, only show warnings/errors
  --version              show version information

Online help: <https://github.com/shadowsocks/shadowsocks>

```
#### （1）使用命令行配置 shadowsocks
```
sslocal -s 11.22.33.44 -p 50003 -k "123456" -l 1080 -t 600 -m aes-256-cfb

```
#### （2）使用JSON文件配置（推荐）
在本地创建个文件shadowsocks.json,(这里我创建在用户目录下 ~/)，内容如下：
```
{
    "server":"11.22.33.44",
    "server_port":50003,
    "local_port":1080,
    "password":"123456",
    "timeout":600,
    "method":"aes-256-cfb"
}

server  你服务端的IP
servier_port  你服务端的端口
local_port  本地端口，一般默认1080
passwd  ss服务端设置的密码
timeout  超时设置 和服务端一样
method  加密方法 和服务端一样
```
再在终端输入 ：sslocal -c ~/shadowsocks.json,结果如下：  
```
INFO: loading config from /home/nwpu/shadowsocks.json
2018-10-25 19:51:42 INFO     loading libsodium from libsodium.so.18
2018-10-25 19:51:42 INFO     starting local at 127.0.0.1:1080

```
到这步并没有完成，还需要在谷歌设置代理


## 二、配置 Proxy SwitchyOmega
下载安装参考[https://github.com/FelisCatus/SwitchyOmega/releases](https://github.com/FelisCatus/SwitchyOmega/releases)   

#### 1、添加代理服务器
点击谷歌右上角  Proxy SwitchyOmega（假设你已经装好）->options->New profile  
如图：  
<img src="{{ '/styles/images/ssr/02.png' | prepend: site.baseurl }}" alt="02"  />

#### 2、设置自动切换模式
Switch rules 切换规则，是指满足这种情景的情况下 使用后面选择的代理服务器上网，可以通过 Rule List Config进行批量导入。  
1、Rule List Format 选 AutoProxy  
2、Rule List URL :[https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt](https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt)   
3、点击 Download Profile Now  
如图：  
<img src="{{ '/styles/images/ssr/03.png' | prepend: site.baseurl }}" alt="03"  />


