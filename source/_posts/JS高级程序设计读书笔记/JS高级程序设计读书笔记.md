---
title: JS高级程序设计读书笔记
date: 2020-05-21 13:47:00
tags: javascript
---

#### 第十七章 事件处理

#### 第十八章  脚本化http

当HTML表单同时包含文件上传元素和其他元素时，浏览器不能使用普通的表单编码而必须使用multipart/form-data的特殊Content-Type来用POST方法提交表单

借助\<script\>发送HTTP请求：JSONP,使用\<script>元素进行Ajax传输，可以使用它们从其他服务器请求数据，包含JSON编码数据的响应体会自定解码

#### 第十九章 Jquery类库

$() 的返回值是一个jQuery对象，jQuery对象是类数组，它们拥有length属性和结余0~length-1之间的数值属性，和querySlectorAll()方法类似，$()返回的类数组对象比querySlectorAll返回的类数组对象更有用。

获取元素的高度，兼容浏览器，$("#sprite").offset(); //获取当前位置



#### 第二十章 客户端存储

##### 20.1localStorage和sessionStorage

浏览器仅仅支持字符串类型数据，如果想要存储和获取其他类型的数据要自己手动编码和解码

```
JSON.stringify() ,JSON.parse()
```

非同源的文档间无法共享sessionStorage的。如果一个浏览器标签包含\<iframe\>元素，所包含的文档是同源的，两者之间共享sessionStorage

`localStorage和存储事件都是采用广播机制的，浏览器会对目前正在访问同样站点的所有串口发送消息`

#### 20.2 cookie

cookie的有效期是Web浏览器的会话期间，一旦用户关闭浏览器，cookie保存的数据就丢失了。`但是 cookie的有效期和sessionStorage的有效期还是有区别的，cookie的作用于并不局限在浏览器的单个窗口中，它的有效期是整个浏览器进程，如果想要延长的话可以设置max-age,但是必须告诉浏览器有效期`。

有的大型网站想要子域之间能够相互共享cookie,比如order.example.com域下的服务器想要读取catalog.example.com域下设置的cookie值，这时候就要设置cookie的domain属性来达到目的，如果catalog.example.com域下的一个页面创建了一个cookie,并将其属性设置成"/",其domain属性设置成".example.com",那么该cookie以及任何其他example.com域下的任何服务器都课件，没有domain，默认是web服务器对的主机名。

cookie还有secure，是一个布尔属性，用来表示cookie的值以何种形式传递，cookie默认是以不安全的形式传递的，一旦cookie被标识为安全的，那就只能当浏览器和服务器通过https或者其他的安全协议连接的时候才能传递他。

保存cookie

由于cookie的（名/值）不允许分号、逗号和空白符，因此，存储钱一般可以采用Javascript核心的全局函数encodeURIComponent()对值进行编码，相应的读取cookie值的时候需要 采取decodeURIComponent()函数解码。

##### cookie相关的存储

实现一个基于cookie的存储API

cookieStorage.js（像localStoage和sessionStrage一样的存储API，基于HTTP cookie实现）

```
function cookieStoage(maxage, path){
	//两个参数基于存储有效期和作用域
	var cookie = (function(){
		var cookie = {}
		var all = document.cookie; // 以大字符串的形式获取所有的cookie信息
		if(all === "")
		return cookie;//返回一个空对象
		var list = all.split(";");//分离键值对
		for(var i=0;i<list.length;i++){
			var cookie = list[i];
			var p = cookie.indexOf("=");
			var name = cookie.substring(0,p); //获取cookie名字
			var value = cookie.substring(p+1);
			value = decodeURIComponent(value);
			cookie[name] = value;    //将名值对存储到对象
		}
		return cookie;	
	}())
	var keys = [];
	for(var key in cookie) keys.push(key);
	
	this.length = keys.length;
	//返回第几个cookie的名字，如果n月结返回null
	this.key = function(n){
		if(n<0||n>= keys.length) return null;
		return keys[n];
	}
	//返回指定名字的cookie值，如果不存在返回null
	this.getItem = function(name){
		return cookie[name] || null;
	}
	this.setItem = function(key,value){
		if(!(key in cookie)){
			keys.push(key)；
			this.length++; //cookie格式加一
		}
		cookie[key] = value;
		var cookie = key +"="+encodeURIComponent(value)
		if(maxage) cookie += ";path=" + path
		//document.cookie属性设置cookie
		document.cookie = cookie;
	};
	//删除指定的cookie
	this.removeItem = function(key){
		if(!(key in cookie)) return ;
		delete cookie[key];
		for(var i = 0;i<keys.length;i++){
			if(keys[i] === key){
				keys.splice(i,1)
				break;
			}
		}
		this.length--;  cookie个数减1
		document.cookie = key +"=;max-age=0";
	};
	this.clear = function(){
		for(var i = 0;i<keys.length;i++)
		document.cookie = keys[i]+"=;max-age = 0";
		//重置所有的内部状态
		cookie = {};
		keys = [];
		this.length = 0;
	}
}

```

