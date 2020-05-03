# this

this的绑定有四种规则。

## 默认
在非严格模式下，默认绑定的this指向全局对象，严格模式下this指向undefined
``` javascript
function foo(){
  console.log(this); // window
}

function bar(){
  "use strict";
  console.log(this); // undefined
}
```
浏览器环境中默认指向window，在严格模式下指向undefined
``` javascript
function foo(){
  console.log(this);
}

function bar(){
  "use strict";
  foo();
}
```
上面仍然输出window全局对象
对于默认绑定来说，决定this绑定对象的是函数体是否处于严格模式

在函数中以函数作为参数传递，例如setTimeOut和setInterval等，这些函数中传递的函数中的this指向，在非严格模式指向的是全局对象。
针对这个情况，我们自己举个例子
``` javascript
var text = 'global';
function foo(fn){
  fn()
}

function say(){
  console.log(this.text);
}

let obj = {
  text: 'obj',
  say: say
}

obj.say(); // obj
foo(obj.say); // global
```
又发现个有意思的事，如果改成
``` diff
+ let text = 'global';
function foo(fn){
  fn()
}

function say(){
  console.log(this.text);
}

let obj = {
  text: 'obj',
  say: say
}

obj.say(); // obj
foo(obj.say); // undefined
```
使用let声明变量后，全局对象window上就没有text了。

## 隐式
函数在调用位置，是否有上下文对象，如果有，那么this就会隐式绑定到这个对象上
``` javascript
var a = 'scope';

function say(){
  console.log(this.a);
}

let obj = {
  a: 'obj',
  say: say
}
// this指向调用函数的对象
obj.say(); // obj

let obj2 = {
  a: 'obj2',
  obj: obj
}
// this指向最后一层调用函数的对象
obj2.obj.say(); // obj

// 隐式绑定丢失
let fn = obj.say;
fn(); // scope
```
当有多层对象嵌套调用某个函数的时候，如 对象.对象.函数,this 指向的是最后一层对象

看一道有意思的题目
``` javascript
function A(name){
  this.name = name || 'world'
}
A.prototype.hello = function(){
  return function(){
    console.log(`hello ${this.name}`)
  }
}
const obj = new A();
const hello = obj.hello();
hello();
```
请问最后在浏览器控制台输出的是什么？
答案是hello。通常正常思维的话，我们都会答hello undefined，其实思路是对的，但有个特殊的点就是，window.name是有默认值的，是一个空字符串，太搞笑了。

## 显式
``` javascript
```
### call
``` javascript
const obj = {
  a: 'obj'
}
function say(){
  console.log(this.a)
}
say.call(obj);
```
如果你把null或者undefined作为this的绑定对象传入call/apply/bind，这些值会在调用时被忽略，实际应用的是默认绑定规则

### apply

### bind

## new
[new的规则](../write/new.md)
``` javascript
```

## 绑定优先级
new > 显示 > 隐式 > 默认

## 箭头函数

## 题目
``` javascript
var length = 10;
function fn() {
    console.log(this.length);
}
 
var obj = {
  length: 5,
  method: function(fn) {
    fn();
    arguments[0]();
  }
};
 
obj.method(fn, 1);
// 输出结果为10, 2
```
第一个输出10，符合函数中传递参数的情况，this指向全局。
第二个输出2有点难理解。arguments为[fn, 1]，arguments是一个类数组，也是一个对象，可看成arguments.fn()，所以this指向了arguments。