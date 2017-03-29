---
layout: post
title:  "ajax readyState的五种状态 [js]"
date:   2016-02-02 12:00:00 +0800
categories: [js]
---

## ajax readyState的五种状态


- 0: (Uninitialized) the send( ) method has not yet been invoked.
 
    0 － （未初始化）还没有调用send()方法

- 1: (Loading) the send( ) method has been invoked, request in progress. 

    1 － （载入）已调用send()方法，正在发送请求
    
- 2: (`Loaded`) the send( ) method has completed, entire response received.

    2 － （载入完成）send()方法执行完成，已经接收到全部响应内容

- 3: (Interactive) the response is being parsed.

    3 － （交互）正在解析响应内容

- 4: (`Completed`) the response has been parsed, is ready for harvesting.

    4 － （完成）响应内容解析完成，可以在客户端调用了
