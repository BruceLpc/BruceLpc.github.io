---
title: ajax 详解
date: 2017-03-17 19:03:03
tags:
---
## 前言
最近面试老被问到什么是Ajax，ajax怎么执行，有几种状态等等，平时都直接用jQuery封装好的，原理也没弄清楚，回答的总是磕磕绊绊，回来又看了一遍高级程序设计，理清了思路，正好整理一下。

那么ajax是什么?
ajax（`Asynchronous Javascript XML`）是指把浏览器原生的通信能力（即向服务器发送http请求）提供给了开发人员，这样开发人员能够额外向服务器请求数据而无须刷新页面，其核心是XMLHttpRequest对象（简称XHR），IE浏览器中是ActiveXObject对象。


## 怎么用？

##### 创建XHR对象:

``` javascript
function createXHR(){
	var xhr;
	if(typeof XMLHttpRequest !='undefined'){
		xhr = new XMLHttpRequest();
	}else if (typeof ActiveXObject !='undefined'){
		xhr = new ActiveXObject();
	}
	return xhr;
}
```

在使用xhr对象时，要调用的第一个方法是open();他接受三个参数：要发送的请求类型，请求的URL和表示是否异步发送的请求布尔值，示例:
``` javascript
xhr.open('get','example.php',false);
```
需要说明：

URL必须跟当前页面的域名、端口和协议一致，否则会报错（这就是传说中的跨域）。
调用open方法并不是真正发送请求，而只是开启一个请求以备用。
要发送请求必须再调用send()方法，send方法接受一个参数，既要向请求主体发送的数据，如果不需要则必须传入null，调用send()方法后请求会立即发送到服务器。

收到响应后，返回的数据会自动填充到xhr对象属性中，相关的属性有：

	responseText:返回的文本
	responseXML: 返回的XML DOM文档
	status:响应的http状态
	statusText：http状态的说明
	readyState: 该属性表示请求/响应过程中的当前活动阶段

	readyState的值有以下几种：
	0：未初始化。尚未调用open()方法
	1：启动。已经调用open()方法，但尚未调用send()方法
	2：发送。已经调用send()方法，但尚未收到响应
	3：接收。已经接受到部分响应数据
	4：完成。已经接受到全部响应数据而且可以在客户端使用了

每当readyState的值变化时都会触发readyStateChange事件，可以利用这个事件来检测每次状态变化后的值，由此确定返回的数据是否可用，代码示例：
``` javascript
var xhr=createXHR();
xhr.onreadyChange=function(){
	if( xhr.status==200 && xhr.status<300&& xhr.readyState==4){
		console.log(xhr.responseText)
	}else{
		console.log('请求失败')
	}
}
xhr.open('post','example.txt',true);
xhr.send(null);
```
** 注意一点：必须在调用open()方法之前绑定onreadyStateChange事件才能保证浏览器兼容性。** 



