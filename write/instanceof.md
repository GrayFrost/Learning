# instanceof

## 定义

我们先看一下MDN对instanceof运算符的描述

> instanceof 运算符用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上

## 例子

``` javascript
function Person() {}
let person = new Person();
person instanceof Person // true
person instanceof Object // true
```

## 实现

``` javascript
function myInstanceof(instance, Con) {
    let left = instance.__proto__;
    let right = Con.prototype;
    while (true) {
        if (left === null) {
            return false;
        } else if (left === right) {
            return true;
        }
        left = left.__proto__;
    }
}
```

在之前我们实现new的时候，发现涉及了原型链的知识。这个章节的instanceof的实现同样涉及到原型链的知识。


