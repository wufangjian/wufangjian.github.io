---
layout: post
title:  "ECMAScript arguments 对象 [js]"
date:   2017-01-15 16:16:16 +0800
categories: [js]
---

# arguments 对象

> 说明： arguments对象，在函数方法内部使用

**一、无需指定参数名称**

``` javascript
function Test(){
    if(arguments[0] === 'a'){
        return true;
    }
    return arguments[0];
}
```


**二、检测参数个数**

``` javascript
function Test(){
    return arguments.length;
}

console.log(Test(1,2,3)); // 3
```

说明：ECMAScript 不会验证传递给函数的参数个数是否等于函数定义的参数个数,开发者定义的函数都可以接受任意个数的参数（根据 Netscape 的文档，最多可接受 255 个），而不会引发任何错误。任何遗漏的参数都会以 undefined 传递给函数，多余的函数将忽略。



**三、模拟函数重载**

方法重载：用同一方法，处理不同类型数据的一种手段，多个重名函数同时存在，具有不同的参数个数/参数类型

java:

``` java
class Person{
    public void method(int a){
        return a;
    }
    
    public void method(int a, int b){
            return a + b;
    }
}

```

js : 中只需要判断传入参数个数就能实现方法的重载

``` javascript
function method(){
    if(arguments.length === 1){
        return arguments[0];
    }
    
    if(arguments.length === 2){
        return arguments[0] + arguments[1];
    }
}
```



**四、arguments对象转换为数组**

4.1 类数组： 

* 拥有数组的一部分属性,如：length

* 也有自己的特有属性, 如：callee [当前函数的引用,只在函数执行时才存在]

说明：类数组除了arguments, var nodeList = document.querySelectAll('div')中的nodeList也是类数组
    
    

4.2 转换为数组：

* 借助数组特性进行转换 Array、slice、splice



``` javascript
// 重点在于: 返回Array类型的对象

Array.call(arguments);
Array.prototype.slice.call(arguments);
Array.prototype.splice.call(arguments, 0);
```

能调用call的只有方法，所以不能用[].call这种形式，得用[].slice。而call的第一个参数表示真正调用slice的环境变为了arrayLike对象。所以就好像arrayLike也具有了数组的方法。

* 遍历

``` javascript
// 遍历是程序开发中万能的方法，高级方法底层往往就是调用的for循环

function A(){
    var ret = [];
    var arr = arguments;
    for(var i = 0, len = arr.length; i < len; i++){
        ret.push(arr[i]);
    }
    return ret;
}
```
