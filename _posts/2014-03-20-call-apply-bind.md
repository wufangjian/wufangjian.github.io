---
layout: post
title:  "call、apply、bind用法 [js]"
date:   2014-03-20 16:16:16 +0800
categories: [js]
---

javascript的一大特点，函数存在 `定义时上下文` `运行时上下文` `上下文可改变` 的概念

## call 、apply

call 和 apply 函数就是为了改变函数的 `运行时上下文` 而存在，也就是改变函数体内部的 this 指向


```javascript
var fruits = function() {}
fruits.prototype = {
    color: 'red',
    say: function(){
        console.log(this.color);
    }
}

var apple = new fruits();

apple.say(); // red

```


banana = {color: 'blue'}; 此时想让bannana也拥有say方法

```javascript
apple.say.call(banana); // blue
apple.say.apple(banana);  // blue

```
此时我们可以看出，call 、apply 可以动态改变this指向，当某个对象不存在某个方法，而另外一个对象有该方法，那么我们可以用apply 和 call去改变该方法的this指向来完成

---

## call 、apply的区别

call 和 apply 的方法作用是完全一致的，他们的区别在于传输参数的方式不同

- call: 枚举每一个参数   用于确定参数个数的场景
- apply: 参数放在一个数组对象里面  用于不确定参数个数的场景

栗子：

```javascript

var func = function(arg1, arg2){
  console.log(arg1 + ' ' + arg2)
     
}

func.call(this, 'arg1', 'arg2') // arg1 arg2
func.apply(this, ['arg1', 'arg2']) // arg1 arg2

```

常见使用场景:

1.数组之间的追加

```javascript

var arr = ['a1', 'a2', {id: 1}, true];
var brr = [1, 2];

Array.prototype.push.apply(arr, brr);

```

2.获取数组中的最大值

```javascript
var arr = [1,2,3,4,5,11,22]
Math.max.apply(Math, arr)
```

3.验证是否是数组

```javascript
function isArray(obj){
  return Object.prototype.toString.call(obj) === '[object Array]'
}
```

4.类数组转换为数组

Javascript中存在一种名为伪数组的对象结构。比较特别的是 arguments 对象，还有像调用 getElementsByTagName , document.childNodes 之类的，它们返回NodeList对象都属于伪数组。不能应用 Array下的 push , pop 等方法。

但是我们能通过 Array.prototype.slice.call 转换为真正的数组的带有 length 属性的对象，这样 domNodes 就可以应用 Array 下的所有方法了。

```javascript

var domNodes = Array.prototype.slice.call(document.getElementsByTagName("*"));

```


---

## call、apply栗子

1.模拟console.log方法打印日志

```javascript
function log(msg) {
  console.log(msg);
}

log('aaa');  // aaa
```

以上例子只能打印一个参数的情况，如果需要打印多个参数？

```javascript
function log(){
  console.log.apply(console, arguments);
}
log(1,2,3);  // 1 2 3
```

在输出值前面添加上 `(app)`

```javascript
function log() {
  var args = [].slice.apply(arguments);
  args.unshift('(app)');
  
  console.log.apply(console, args);
}
log(1, 2, 3);
```


---

## bind

bind()方法会创建一个新函数，称为绑定函数，当调用这个绑定函数时，绑定函数会以创建它时传入 bind()方法的第一个参数作为 this，传入 bind() 方法的第二个以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数的参数来调用原函数。

常见 通过缓存中间变量来改变上下文

```javascript

var foo = {
    bar : 1,
    eventBind: function(){
        var _this = this;
        $('.someClass').on('click',function(event) {
            /* Act on the event */
            console.log(_this.bar);     //1
        });
    }
}

```


使用bind()后:

```javascript

var foo = {
    bar : 1,
    eventBind: function(){
        $('.someClass').on('click',function(event) {
            /* Act on the event */
            console.log(this.bar);      // 1
        }.bind(this));
    }
}
```

在上述代码里，bind() 创建了一个函数，当这个click事件绑定在被调用的时候，它的 this 关键词会被设置成被传入的值（这里指调用bind()时传入的参数）。因此，这里我们传入想要的上下文 this(其实就是 foo )，到 bind() 函数中。然后，当回调函数被执行的时候， this 便指向 foo 对象。


再来一个简单的栗子：


```javascript
var bar = function(){
  console.log(this.x);
}

var foo = {
  x:3
}

bar(); // undefined
var func = bar.bind(foo);
func(); // 3

```

这里我们创建了一个新的函数 func，当使用 bind() 创建一个绑定函数之后，它被执行的时候，它的 this 会被设置成 foo ， 而不是像我们调用 bar() 时的全局作用域。

注意：多次调用bind只有第一次有效

```javascript

var bar = function(){
    console.log(this.x);
}
var foo = {
    x:3
}
var sed = {
    x:4
}
var func = bar.bind(foo).bind(sed);
func(); // 3
  
var fiv = {
    x:5
}
var func = bar.bind(foo).bind(sed).bind(fiv);
func(); // 3
```

在Javascript中，多次 bind() 是无效的。更深层次的原因， bind() 的实现，相当于使用函数在内部包了一个 call / apply ，第二次 bind() 相当于再包住第一次 bind() ,故第二次以后的 bind 是无法生效的。

---

## call 、apply、bind的区别

当你希望改变上下文环境之后并非立即执行，而是回调执行的时候，使用 bind() 方法。而 apply/call 则会立即执行函数。

```javascript
var obj = {
    x: 81,
};
  
var foo = {
    getX: function() {
        return this.x;
    }
}
  
console.log(foo.getX.bind(obj)());  //81
console.log(foo.getX.call(obj));    //81
console.log(foo.getX.apply(obj));   //81
```

---

## 总结

*apply 、 call 、bind 三者都是用来改变函数的this对象的指向的；*
*apply 、 call 、bind 三者第一个参数都是this要指向的对象，也就是想指定的上下文；*
*apply 、 call 、bind 三者都可以利用后续参数传参；*
*bind 是返回对应函数，便于稍后调用；apply 、call 则是立即调用 。*


