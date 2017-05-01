---
layout: post
title:  "WebPagetest [性能]"
date:   2017-05-01 18:48:00 +0800
categories: [性能]
---

> WebPagetest 是一款 Web 应用程序，它将一个 URL 以及一系列配置参数作为输入，并对那个 URL 运行性能测试。WebPagetest 可配置的参数非常多，范围也非常广

**目录**

---

[]()

[]()

[]()

---


**1.advanced (高级的)**

- 1、Stop Test at Document Complete (文档完成时停止测试运行)

*可以通知我们 document.onload 事件是何时出发的，而不是当前所有页面资源都加载完成之后再通知我们*

- 2、Disable JavaScript (禁用javascript)

- 3、Clear SSL Certificate Caches (清除 SSL 证书缓存)

- 4、Ignore SSL Certificate Errors (忽略 SSL 认证错误)

*否则测试可能无法正常运行，因为这种情况下最终用户需要出示证书后会话才能继续进行，否则会话将终止*

- 5、Disable Compatibility View (IE Only) (禁用兼容性视图)

- 6、Capture network packet trace (tcpdump) (捕获网络包跟踪)

- 7、Save response bodies (保存返回响应体)

- 8、Preserve original User Agent string (保留原有用户代理字符串)

*保留用户原有的运行测试的浏览器的代理字符串，而不是添加一个字符串来将该访问识别为一个 WebPagetest 测试*

**2.chrome (特定的高级设置)**

> 模拟不同的手机（xiaomi、huawei等），不同的端（ios、android），比如在 iOS下某些功能存在，android下面不存在


**3.Auth (认证)**

> 如果 Web 网站使用 HTTP 认证才能访问，可以在 Auth（认证）选项卡中指定证书，要记住小心谨慎。(推荐使用创建一个仅用于测试的有权限限制的账号进行测试)


**4.script (脚本)**

> 测试一些非常特殊的场景，例如：仅当某些特殊事件触发时才对这些特性进行性能测试。 你可以运行一个复杂的测试（访问多个URL、发送click、key事件给DOM，提交表单数据，执行特殊的 JavaScript，还要更新 DOM）甚至（修改 HTTP 请求设置，以实现诸如设置特定 cookie、主机IP设置以及用户代理变更等事情）


**5.Block (分块)**

> 允许我们对`请求的内容进行分块`，这对于我们比较有没有广告、有没有javascript、以及有没有图片文件的测试结果是非常有用的

如果我们想要通过编写脚本将一个网站的所有 png 文件分块，可以这么做(在script选项卡中编写包含分块命令的脚本完成此功能)：

```
block .png
navigate http://www.tom-barker.com
```


在瀑布图和屏幕截图上方的表格中是页面级别指标(page level metrics)，包括`整页加载时间`、`第一字节加载所需时间`、`第一项内容加载所需时间`、`页面中 DOM元素的数量`、`document.onload事件触发事件`，`页面所有元素加载花费时间`，以及为了`完成页面绘制所需请求的 HTTP请求次数`

![](/static/img/2017/webpagetest01.png)

















