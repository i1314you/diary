---
title: ajax
date: 2022-10-14 19:21:47
tags:
---

#	XML简介

可扩展标记语言，用来传输和存储数据，不同于html是预定义标签，不过都差不多用``json``来取代了

# HTTP

请求报文 ：请求行：GET  ``url``参数  HTTP/1.1（版本），请求头：如Host: atguigu.com，空格，请求体 :  get没有请求体，post有

相应报文：相应行： HTTP/1.1  200    OK，相应头:跟请求头的格式差不多，空格，响应体：服务器返回给我们的数据

# Express

基于Node.js开发的Web开发框架，用来写服务器的

安装：``node -v    ,   npm init   ,     npm i express``

基本使用：

``          const express = require('express');``

​		``	const app = new express();``

​	``		app.get('/server',(request,response)=>{``

``	response.send('HELLO EXPRESS')			``

``	})``

``app.listen(8000,()=>{``

``console.log("服务已启动")``

``})``

# ajax

``const xhr = new XMRHttpRequest();//创建对象``



下面这段代码是超时与网络异常的处理

``xhr.timeout = 2000;``

``xhr.ontimeout = function(){``

  ``alert('网络异常')``

}

``xhr.onerror = function(){``

  ``alert('网络出现问题')``

}



``xhr.open('GET','http://127.0.0.1:8000/server');//初始化，设置请求方法和url``

``xhr.send();   //发送``

``// readyState 总共有5个值：0,1,2,3,4  值为4的时候表示服务器将全部数据发送过来了``

``xhr.onreadystatechange = function(){``

 `` if(xhr.readyState === 4 && xhr.status == 200){``

 ``   console.log(xhr.status);//状态码``

``    console.log(xhr.statusText);//状态字符串``

``    console.log(xhr.response);//响应体``

  }else{

  }

}

使用``xhr.abort()``取消本次网络请求

重复请求问题

![](C:\Users\33834\Desktop\博客图片\ajax重复请求问题.jpg)

# 解决跨域

JSONP 只支持get请求

有一些标签具有跨域能力，如``img,link,iframe,script``JSONP就是借助于script

CORS  不需要客户端操作，在服务器进行，其实就是设置请求头就行