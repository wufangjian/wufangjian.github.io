---
layout: post
title: "XHR Level2 / IE XDR (XDomainRequest) 实现AJAX跨域 [性能]" 
date:   2014-10-10 20:16:16 +0800
categories: [性能]
---
> 遇到AJAX跨域需求时都会采用服务器端代理，或者改用 JSONP,  window.name，postMessage() 等方法模拟，但都不是很纯粹的AJAX解决方案。今天学到了新的实现跨域的方法


关于跨域可以参考[前端解决跨域问题的8种方案](http://blog.csdn.net/joyhen/article/details/21631833)

## 一、XHR Level2

HTML5中 XMLHttpRequest Level 2 的推出

可以通过在返回的HTTP请求头中加入 `Access-Control-Allow-Origin` 的设置，让浏览器支持对不同域的AJAX请求。

这个情况下前端AJAX的代码不用做任何更改，只需要在服务端的返回中设置以下头信息即可:

- Access-Control-Allow-Origin: * //*代表任何域。也可以指定地址
- Access-Control-Allow-Methods: POST,GET //支持的方法

```
header( " Access-Control-Allow-Origin: * " );
header( " Access-Control-Allow-Methods: POST,GET " );
```

## 二、XDR (IE 8-9 Only)

对于XHR2，IE浏览器的支持是IE10以上。但是IE早在IE8时就推出了 XDomainRequest 对象进行跨域操作，一直沿用到IE10才被取代掉。因此在IE8,IE9中应该使用 XDomainRequest (XDR)来实现。

XDR在创建时是通过 new XDomainRequest() 进行创建。其他操作和XHR有细微差别。比如open方法只有method和url两个参数，XDR只支持异步不支持同步操作。

```javascript
// 1. Create XDR object: 
var xdr = new XDomainRequest(); 
// 2. Open connection with server using GET method:
xdr.open("get", "http://www.contoso.com/xdr.aspx");
// 3. Send string data to server:
xdr.send();
```

另外使用XDR时也需要服务器端设置上面提到的 Access-Control-Allow-Origin 头信息。