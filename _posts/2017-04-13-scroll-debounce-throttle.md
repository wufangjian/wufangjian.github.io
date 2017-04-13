---
layout: post
title: "debounce throttle [性能]" 
date:   2017-04-13 15:47:16 +0800
categories: [性能]
---
> 当用户浏览网页时，拥有平滑滚动经常是被忽视但却是用户体验中至关重要的部分。当滚动表现正常时，用户就会感觉应用十分流畅，令人愉悦，反之，笨重不自然卡顿的滚动，则会给用户带来极大不舒爽的感觉。这里有两个重要的概念`debounce（防抖）`，`throttle（节流）`

以下场景往往由于事件频繁被触发，因而频繁执行DOM操作、资源加载等重行为，导致UI停顿甚至浏览器崩溃。

- window对象的resize、scroll事件

- 拖拽时的mousemove事件

- 射击游戏中的mousedown、keydown事件

- 文字输入、自动完成的keyup事件


## debounce（防抖）

> 如果用手指一直按住一个弹簧，它将不会弹起直到你松手为止。

也就是说当调用动作n毫秒后，才会执行该动作，若在这n毫秒内又调用此动作则将重新计算执行时间。

```javascript

/**
* 空闲控制 返回函数连续调用时，空闲时间必须大于或等于 idle，action 才会执行
* @param idle   {number}    空闲时间，单位毫秒
* @param action {function}  请求关联函数，实际应用需要调用的函数
* @return {function}    返回客户调用函数
*/
debounce(idle,action);
```

```javascript

var debounce = function(idle, action){
  var last;
  return function(){
    var ctx = this, args = arguments;
    clearTimeout(last);
    last = setTimeout(function(){
        action.apply(ctx, args);
    }, idle);
  }
}
```

---

## throttle（节流）

> 如果将水龙头拧紧直到水是以水滴的形式流出，那你会发现每隔一段时间，就会有一滴水流出

防抖函数确实不错，但是也存在问题，譬如图片的懒加载，我希望在下滑过程中图片不断的被加载出来，而不是只有当我停止下滑时候，图片才被加载出来。又或者下滑时候的数据的 ajax 请求加载也是同理。

也就是会说预先设定一个执行周期，当调用动作的时刻大于等于执行周期则执行该动作，然后进入下一个新周期。

```javascript

/**
* 频率控制 返回函数连续调用时，action 执行频率限定为 次 / delay
* @param delay  {number}    延迟时间，单位毫秒
* @param action {function}  请求关联函数，实际应用需要调用的函数
* @return {function}    返回客户调用函数
*/
throttle(delay,action);
```

```javascript

var throttle = function(delay, action){
  var last = 0;
  return function(){
    var curr = new Date();
    if (curr - last > delay){
        action.apply(this, arguments);
        last = curr;
    }
  }
}
```




