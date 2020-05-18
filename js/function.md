# 函数式编程

## 柯里化
``` javascript
function curry(fn){
  let argArr = [];
  return function F(...args){
    argArr = [...argArr, ...args]
    if(argArr.length >= fn.length){
      fn.apply(this, argArr);
    }else{
      return F;
    }
  }
}
```
核心思想是闭包存储变量的数量，然后通过与函数的length来比较，function.length可以拿到参数的数量。达到原始函数的变量数量时，再调用函数，否则返回一个函数，继续接收参数。

``` javascript
function add(a, b, c){
  console.log(a + b + c);
}

let myAdd = curry(add);
myAdd(1)(2)(3);
```
没想到现在的我也能够写出个简单的来了，开心。

## compose

首先我们举个例子，假设我们需要对两个数相加，然后将加完的和进行乘法运算，最后将进行乘法后的值进行字符串拼接。
``` javascript
function add(num1, num2) {
    return num1 + num2;
}

function multi(num) {
    return num * num;
}

function toStr(num) {
    return num + " Hello world";
}
```
我们将每个功能函数拆分，拆成三个模块。通常我们的调用方式如下
``` javascript
let str = toStr(multi(add(1,2))); // 9 Hello world
```
我们发现这种调用方式的缺点就是，如果函数多了，就会有一层层的嵌套。针对这种下一个调用函数的入参依赖上一个函数出参的调用方式，我们可以使用compose解决。compose函数可以将嵌套执行的函数进行平铺。
我先按照我的理解写一个
``` javascript
function compose(...funcs) {
    return function (...args) {
        return funcs.reduce((res, fn) => {
            return typeof res === 'function' ? fn(res(...args)) : fn(res)
        });
    };
}
```
compose函数接收的是一系列方法作为参数，然后返回一个函数，这个函数接收最开始需要计算的参数值。然后内部使用reduce来执行一系列的参数，因为没有默认值，第一次的时候，reducce回调函数的第一个参数是一个函数，此后的迭代才是一个值。当然这只是compose实现的一种思路。  
如果需要按照网上说的从右往左执行，改成reduceRight就可以

``` javascript
let str = compose(add, multi, toStr)(1, 2); // 9 Hello world
```

看一题面试题：将数组扁平化并去除其中重复数据，最终得到一个升序且不重复的数组
``` javascript
function unique(arr) {
    return [...new Set(arr)];
}
function flatten(arr) {
    return arr.reduce((prev, cur) => {
        Array.isArray(cur) ? prev.concat(flatten(cur)) : prev.push(cur);
        return prev;
    }, []);
}
function sort(arr) {
    return arr.sort((a, b) => a - b);
}

let arr = compose(flatten, unique, sort)([1, 5, 7, 5, 8, 3, 8, 2, 9, 6]);
```

redux里的compose实现起来就很精巧
``` javascript
```

