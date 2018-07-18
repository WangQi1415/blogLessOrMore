---
layout: post
title:  "WebSocket使用教程"
date:   2018-07-09 13:31:01 +0800
categories: 教程
tag: WebSocket
---

* content
{:toc}


参考博客： <https://blog.csdn.net/u014316363/article/details/43408325 >
## 1、 为什么要使用websocket
传统的浏览器，是我们在浏览器端向服务器端请求数据，服务器端再将数据发送过来，随着对实时通信的需求，我们需要当服务器端的数据发生变化时，便将数据推送到浏览器端。这时我们想到的办法是建立长传机制，在服务器仍然有数据发送的时候，服务器与浏览器之间保持http连接，但是http连接的开销过大， 每次都需要发送http头和cookie数据 ，这无疑大大的加大了实际发送给服务器的数据量。为了解决这一问题，国际互联网工程任务组（The Internet Engineering Task Force，简称 IETF）设计了新的标准 也就是websocket，详见： [JSR 356, Java API for WebSocket](http://www.oracle.com/technetwork/articles/java/jsr356-1937161.html)
## 2、 websocket 是如何进行工作的呢
客户端建立websocket连接是通过 websocket握手过程建立的  
WebSocket握手过程  
1.  首先是由客户端发送常规的HTTP请求给服务器端，这个请求中包含了升级的头部，它告诉了服务器  希望建立websocket连接  
以下为请求建立连接的头部
<pre><code>
GET ws://websocket.example.com/ HTTP/1.1
Origin: http://example.com
Connection: Upgrade
Host: websocket.example.com
Upgrade: websocket
</code></pre>
值得注意的是websocket使用的是  ws的方案，而wss是类似于 https一样的更为安全的方案

2. 如果服务器支持websocket协议 并且同意与浏览器建立websocket连接   则响应该请求
<pre><code>	
	HTTP/1.1 101 WebSocket Protocol Handshake
	Date: Wed, 16 Oct 2013 10:07:34 GMT
	Connection: Upgrade
	Upgrade: WebSocket
</code></pre>
这样就建立了像http一样的连接 并且可以进行发送数据  与http连接相似的是  他们都是建立在tcp/ip连接上  

那怎么可以保证websocket传输的可靠性呢？  
为了保证这些信息可以正确的被重构，每一个帧都有4-12个字节的数据作为前缀，利用这种基于帧的数据传送，可以有效的减少无用数据的传送。  
如图：一个服务器可以连接好几个客户端  客户端可以用实现符合RFC 6455规范  与服务器端进行通信  
<img src="{{ '/styles/images/websocket/websocket_conmu.png' | prepend: site.baseurl }}" alt="websocket_commu" width="500" />

## 3、 编写第一个Demo
#### a 、热身
（1）建立连接  
使用js建立连接  
<pre><code>var ws = 
			new WebSocket('ws://localhost:8080/websocketdemo02/websocket');</code></pre>
可以将其放在button里面,手动点击建立连接即封装成一个函数，也可以放在js代码段的最前面，页面加载时便建立了连接。  
服务器端实现：  
<pre><code>@ServerEndpoint(value="/chat",encoders=MessageEncoder.class,decoders=MessageDecoder.class)
public class MyServer {
}</code></pre>
可以通过ws://localhost:8080/项目名称/chat 与其建立连接  
若在其他主机上 则可以通过 IP地址进行连接   var ws=new WebSocket(‘ws://XXXX.XXXX.XXXX.XXXX/项目名称/chat’)  
（2）常用方法说明  
onOpen()  当连接建立成功后调用该函数；  
	服务器端:被@OnOpen()注解的函数****  
<pre><code>@OnOpen
	public void onOpen(Session session) {
		System.out.println(String.format("%s jioned the chat room.",session.getId()));
		peers.add(session);
	}</code></pre>
客户端：
<pre><code>ws.onopen=function(event){

}</code></pre>
需要注意的是  websocket是事件驱动的API   我们既可以采用注解，也可以采用接口驱动 这里我们采用接口驱动  
		
onMessage()  当接收到信息时自动调用该函数；  
	服务器端：被@OnMessage注解的函数  
<pre><code>@OnMessage
	public void onMessage(String str,Session session) throws IOException,EncodeException, DecodeException{
		//broadcast the message  除了自己之外的别人都接受到消息
		System.out.println(str);
			for(Session peer:peers) {
			if(!session.getId().equals(peer.getId())) {
				peer.getBasicRemote().sendText(str);
			}
		}
	}</code></pre>
static Set<Session> peers=Collections.synchronizedSet(new HashSet<Session>());可以记录所有的连接到该服务端的 session  
客户端 : 
<pre><code>//接收到消息的回调方法
	ws.onmessage=function(event){
		//alert("onmessage");//有消息发送过来时才调用该函数
		console.log(event.data);//
		var obj=JSON.parse(event.data);
		$("#ct").html(obj.message);
	}</code></pre>
OnClose() 函数关闭时自动调用该函数；  
	服务器端：被 @OnClose注解的函数  
<pre><code>@OnClose
	public void OnClose(Session session) throws IOException,EncodeException{
		System.out.println(String.format("%s left the chat root.",session.getId()));
		peers.remove(session);
		
		for(Session peer:peers) {
			Message message=new Message();
			message.setSender("server");
			message.setContent(String.format("%s left the chat room",(String)session.getUserProperties().get("user")));
			message.setRecieved(new Date());
			peer.getBasicRemote().sendObject(message);
		}
	}</code></pre>

客户端：
<pre><code>/连接关闭的回调方法
      websocket.onclose = function () {
          setMessageInnerHTML("WebSocket连接关闭");
      }</code></pre>
 onError() 连接发生错误时调用该函数。  
服务器端：  
<pre><code>@OnError
	public void onError () {
	}</code></pre>
客户端：  
<pre><code>ws.onerror=function(event){
		alert("erorr");
	}</code></pre>
（3）发送消息  
在js中发送字符串  
<pre><code>exampleSocket.onopen = function (event) {
  exampleSocket.send("Here's some text that the server is urgently awaiting!"); 
};</code></pre>  


在js中通过websocket发送JSON格式的数据  
<pre><code>function sendText() {
  // Construct a msg object containing the data the server needs to process the message from the chat client.
  var msg = {
    type: "message",
    text: document.getElementById("text").value,
    id:   clientID,
    date: Date.now()
  };

  // Send the msg object as a JSON-formatted string.
  exampleSocket.send(JSON.stringify(msg));
  
  // Blank the text input element, ready to receive the next line of text from the user.
  document.getElementById("text").value = "";
}</code></pre>
Js中对JSON格式的数据进行解析  
<pre><code>exampleSocket.onmessage = function(event) {
  var f = document.getElementById("chatbox").contentDocument;
  var text = "";
  var msg = JSON.parse(event.data);
  var time = new Date(msg.date);
  var timeStr = time.toLocaleTimeString();
  
  switch(msg.type) {
    case "id":
      clientID = msg.id;
      setUsername();
      break;
    case "username":
      text = "<b>User <em>" + msg.name + "</em> signed in at " + timeStr + "</b><br>";
      break;
    case "message":
      text = "(" + timeStr + ") <b>" + msg.name + "</b>: " + msg.text + "<br>";
      break;
    case "rejectusername":
      text = "<b>Your username has been set to <em>" + msg.name + "</em> because the name you chose is in use.</b><br>"
      break;
    case "userlist":
      var ul = "";
      for (i=0; i < msg.users.length; i++) {
        ul += msg.users[i] + "<br>";
      }
      document.getElementById("userlistbox").innerHTML = ul;
      break;
  }
  
  if (text.length) {
    f.write(text);
    document.getElementById("chatbox").contentWindow.scrollByPages(1);
  }
};</code></pre>
在java后台服务器端向客户端发送数据  
<pre><code>方法一：
@OnMessage
public String myOnMessage (String txt) {
   return txt.toUpperCase();
}
如果@OnMessage 注解的方法返回值不为空  返回的内容将会被发送


方法二：  
RemoteEndpoint.Basic other = session.getBasicRemote();
other.sendText ("Hello, world");

发送对象时   sendObject   ……..</code></pre>

#### b、第一个demo
第一个demo我们编写一个群聊系统我们需要实现的功能是  当一个客户端发送消息，其它的客户端都可以收到。 

* 创建消息实体类
* 消息转换为json类
* 解析json为消息实体类
* 服务器类
* 客户端

 项目源码地址  <https://github.com/WangQi1415/WebSocket/ >
