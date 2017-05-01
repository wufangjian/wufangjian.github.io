---
layout: post
title:  "WebPagetest [性能]"
date:   2017-05-01 18:48:00 +0800
categories: [性能]
---

> WebPagetest 是一款 Web 应用程序，它将一个 URL 以及一系列配置参数作为输入，并对那个 URL 运行性能测试。WebPagetest 可配置的参数非常多，范围也非常广

**目录**

---

[1 性能测试](#一性能测试)

[2 与性能相关的几个重要概念](#二与性能相关的几个重要概念)

[3 webpagetest 参数设置](#三webpagetest-参数设置)

---


# 一、性能测试

在瀑布图和屏幕截图上方的表格中是页面级别指标(page level metrics)，包括 ：

- 整页加载时间 (Load Time)

- 第一字节加载所需时间 (First Byte)

- 第一项内容加载所需时间 (Start Render)

- 页面可见部分的平均时间 (`Speed Index`)

- 页面的可交互时间 (`Interactive`)

- dom ready时间 (`Document complete`)

- 页面所有元素加载花费时间 (`Fully Loaded`)

- 完成页面绘制所需请求的 HTTP请求次数 (Requests)

## 1.1 summary: 摘要

![](/static/img/2017/webpagetest/01summary.png)

Waterfall: 页面 `首次加载视图` 和 `缓存重复加载视图` 的瀑布图

Screen Shot: 对应的视频截图

---

摘要页面提供了所有数据的高度概括,以下是二级页面：

## 1.2 Details: 详情

![](/static/img/2017/webpagetest/02details.png)


---

## 1.3 Performance Review:  性能查看

![](/static/img/2017/webpagetest/03performancereview.png)

显示了每一项内容在各项标准下的表现

- Keep Alire (保持有效)

- Gzip Text (Gzip文本)

- Compress Images (图片压缩)

- Progressive (渐进增强)

- Cache Static (缓存统计)

- CDN detected (CDN检测)

---

## 1.4 Content Breakdown: 内容分解

![](/static/img/2017/webpagetest/04contentbreakdown.png)

---

## 1.5 Domains: 域

![](/static/img/2017/webpagetest/05domains.png)

---

## 1.6 Content Breakdown: 内容分解

![](/static/img/2017/webpagetest/06processingbreakdown.png)

---

## 1.7 Screen Shot: 视频截图

![](/static/img/2017/webpagetest/07screenshot.png)


---

# 二、与性能相关的几个重要概念

## 2.1 Speed Index

定义：速度索引是显示页面的可见部分的平均时间。 它以毫秒为单位，取决于 `浏览器可视区大小`。

---

## 2.2 Start Render

定义：顾名思义指的是浏览器开始渲染的时间，从用户角度出发则可以定义为用户在页面上看到的第一个内容的时间

用户体验：该时间决定着用户对页面的`第一体验时机`，如果时间越短给用户的体验则是页面速度越快，这样用户等待其余内容展现的耐心也会更大一些。如果时间过长，则用户会在长时间内面对的都是一个空白的页面，这对用户的耐心将是一个考验。总的来说，`Start Render时间是越短越好，而且是非常关键的指标`

影响因素：`Time To Start Render = TTFB + TTDD + TTHE`

```
TTFB(Time To First Byte)：发起请求到服务器返回数据的时间

TTDD(Time To Document Download)：从服务器加载HTML文档的时间

TTHE(Time To Head End)：HTML文档头部解析完成所需要的时间

通过以上公式可以看到Start Render主要受以下因素影响(开发人员可控)：

(1) 服务器响应时间

(2) HTML文档的大小

(3) Head中资源使用情况
```


---

## 2.3 DOM Ready

定义：页面解析完成的时间，在高级浏览器里有对应的DOM事件 - `DOMContentLoaded`

> 该事件在文档解析完成时会触发。那么文档解析到底包括哪些操作呢？虽然暂不能给出一个完全的答案，但文档的解析至少应该包括以下操作：`HTML文档分析` 以及 `DOM树的创建`、 `外链脚本的加载`、 `外链脚本的执行` 以及 `内联脚本的执行`，但是不包括图片、iframe等其它资源的加载。正因为如此，该事件触发的时机一般比window.onload要早，而且是在所有DOM元素都可以操作之时。


用户体验：DOM Ready 指标并不直接影响感官体验，往往影响的是交互功能何时可用

> 由于DOMContentLoaded事件触发时是所有DOM节点可以进行操作的时候，比如添加事件、增删改节点等，因此用JavaScript实现的一些交互功能往往会在DOMContentLoaded事件中去初始化，也只有在DOMContentLoaded事件触发后这项功能才可用，DOM Ready时间如果过长的话，用户会发现页面已经出来了，但是很多功能却是不可用的，也许点击某个链接会跳到页面顶部。因此， DOM Ready 时间也是越短越好，这样交互功能才能尽早可用

影响因素：`Time To Dom Ready = TTSR + TTDC + TTST`

```
TTSR(Time To Start Render)：浏览器开始渲染的时间

TTDC(Time To Dom Created)：DOM树创建所耗时间

TTST(Time To Script)：BODY中所有脚本加载和执行的时间

通过以上公式可以看到Start Render主要受以下因素影响(开发人员可控)：

(1) DOM结构的复杂程度

(2) BODY中脚本使用情况
```

说明：通过对一些实际监控数据的分析得出，在一个通过正常方式加载脚本的页面中，脚本的加载和执行时间往往能占DOM Ready时间的50%左右，由此可见脚本的使用对DOM Ready指标的影响如何的显著！因此，对DOM Ready指标的优化也应该着重从脚本的使用入手

---

## 2.4 page load 

定义：window.onload事件触发的时间

> 与DOM Ready时间相比，Page Load的时间往往要更靠后一些，因为Page Load不仅仅是HTML文档解析完毕还包括了所有资源加载所需要的时间，例如图片资源的加载、iframe的加载等

用户体验：

> window.onload事件触发的时间(Page Load)也就是所有资源加载完成的时间，该时间越长意味着用户需要等待越久才能看见某些内容，例如图片，这些内容也许并不是最重要的，但这是一个完整页面的组成部分，这部分内容如果加载过慢，也会在一定程度上影响用户对页面完整性的体验。


影响因素：

1) 因为是在 dom ready 之后触发，因此 dom ready 指标的影响因素也会影响page load

2) 图片、iframe等资源越多，page load 的加载时间也会越长

---

## 2.5 TTI (time to interact)

定义：页面可交互的时间

> 核心功能的定义则是随着页面的不同而不同，例如对于百度首页而言，最为关键的就是搜索框出现的时间、而对于一些购物网站的商品详情页最关键的是购买功能可用和描述出现的时间。而目前的实际情况，TTI大都等于DOM Ready时间，因为不论交互功能是否重要，相关的Javascript都会在DOM Ready 后才进行初始化和绑定，而实际上 `TTI是可以更早的`

用户体验：

TTI 的长短对于用户体验的影响是十分重要的，它影响着用户对核心功能的使用


影响因素：

1) StartRender时间(只有内容渲染出来了，才可以谈交互，因此渲染时间的快与慢会直接影响TTI时间)

2) 核心功能相关HTML元素的显示时间 (决定着核心功能可见的时间)

3) 核心功能相关Javascript功能的绑定时间 (决定着核心 Javascript 功能可交互的时间)


---


# 三、webpagetest 参数设置

## 3.1 advanced (高级的)

- 1、Stop Test at Document Complete (文档完成时停止测试运行)

可以通知我们 document.onload 事件是何时出发的，而不是当前所有页面资源都加载完成之后再通知我们

- 2、Disable JavaScript (禁用javascript)

- 3、Clear SSL Certificate Caches (清除 SSL 证书缓存)

- 4、Ignore SSL Certificate Errors (忽略 SSL 认证错误)

否则测试可能无法正常运行，因为这种情况下最终用户需要出示证书后会话才能继续进行，否则会话将终止

- 5、Disable Compatibility View (IE Only) (禁用兼容性视图)

- 6、Capture network packet trace (tcpdump) (捕获网络包跟踪)

- 7、Save response bodies (保存返回响应体)

- 8、Preserve original User Agent string (保留原有用户代理字符串)

保留用户原有的运行测试的浏览器的代理字符串，而不是添加一个字符串来将该访问识别为一个 WebPagetest 测试

## 3.2 chrome (特定的高级设置)

模拟不同的手机（xiaomi、huawei等），不同的端（ios、android），比如在 iOS下某些功能存在，android下面不存在


## 3.3 Auth (认证)

如果 Web 网站使用 `HTTP 认证`才能访问，可以在 Auth（认证）选项卡中指定证书，要记住小心谨慎。(推荐使用创建一个仅用于测试的有权限限制的账号进行测试)


## 3.4 script (脚本)

测试一些`非常特殊的场景`，例如：仅当某些特殊事件触发时才对这些特性进行性能测试。 你可以运行一个复杂的测试（访问多个URL、发送click、key事件给DOM，提交表单数据，执行特殊的 JavaScript，还要更新 DOM）甚至（修改 HTTP 请求设置，以实现诸如设置特定 cookie、主机IP设置以及用户代理变更等事情）


## 3.5 Block (分块)

允许我们对`请求的内容进行分块`，这对于我们比较有没有广告、有没有javascript、以及有没有图片文件的测试结果是非常有用的

如果我们想要通过编写脚本将一个网站的所有 png 文件分块，可以这么做(在script选项卡中编写包含分块命令的脚本完成此功能)：

```
block .png
navigate http://www.tom-barker.com
```


