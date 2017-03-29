---
layout: post
title:  "ajax readyState的五种状态 [js]"
date:   2016-02-02 12:00:00 +0800
categories: [js]
---

## ajax readyState的五种状态

先看看原生js封装的ajax

```javascript
function ajax(method, url, data, success) {
    var xhr = null;
    try {
        xhr = new XMLHttpRequest(); // 1.创建xhr对象 
    } catch (e) {
        xhr = new ActiveXObject('Microsoft.XMLHTTP');
    }
    
    if (method == 'get' && data) {
        url += '?' + data;
    }
    
    xhr.open(method,url,true); // 2.设置请求信息 
    if (method == 'get') {
        xhr.send(); // 3.提交请求
    } else {
        xhr.setRequestHeader('content-type', 'application/x-www-form-urlencoded');
        xhr.send(data);
    }
    
     // 4.等待服务器返回内容
    xhr.onreadystatechange = function() {
        
        if ( xhr.readyState == 4 ) {
            if ( xhr.status == 200 ) {
                success && success(xhr.responseText); //如果函数存在就执行后面的  &&的执行过程就是前面的是真，才执行后面的。
            } else {
                alert('出错了,Err：' + xhr.status);
            }
        }
        
    }
}
```

- 0: (Uninitialized) the send( ) method has not yet been invoked.
 
    （未初始化）此阶段确认XMLHttpRequest对象是否创建，并为调用open()方法进行未初始化作好准备。值为0表示对象已经存在，否则浏览器会报错－－对象不存在。

- 1: (Loading) the send( ) method has been invoked, request in progress. 

    （载入）此阶段对XMLHttpRequest对象进行初始化，即调用open()方法，根据参数(method,url,true)完成对象状态的设置。并调用send()方法`开始向服务端发送请求`。值为1表示正在向服务端发送请求。 
    
- 2: (`Loaded`) the send( ) method has completed, entire response received.

    （载入完成）此阶段接收服务器端的响应数据。但获得的还只是服务端响应的原始数据，并不能直接在客户端使用。值为2表示`已经接收完全部响应数据`。并为下一阶段对数据解析作好准备。 

- 3: (Interactive) the response is being parsed.

    （交互）此阶段解析接收到的服务器端响应数据。即根据服务器端响应头部返回的MIME类型把数据转换成能通过responseBody、responseText或responseXML属性存取的格式，为在客户端调用作好准备。状态3表示`正在解析数据`。 

- 4: (`Completed`) the response has been parsed, is ready for harvesting.

    （完成）此阶段确认全部数据都已经解析为客户端可用的格式，解析已经完成。值为4表示数据解析完毕，可以通过XMLHttpRequest对象的相应属性取得数据。 


## onreadystatechange 事件

当请求被发送到服务器时，我们需要执行一些基于响应的任务。

每当 readyState 改变时，就会触发 onreadystatechange 事件。

readyState 属性存有 XMLHttpRequest 的状态信息。

下面是 XMLHttpRequest 对象的三个重要的属性：
```
onreadystatechange 
    存储函数（或函数名），每当 readyState 属性改变时，就会调用该函数。

readyState
    0: 请求未初始化
    1: 服务器连接已建立
    2: 请求已接收
    3: 请求处理中
    4: 请求已完成，且响应已就绪
    

status
    200: "OK"
    404: 未找到页面
```

注释：`onreadystatechange` 事件被触发 5 次（0 - 4），对应着 readyState 的每个变化。