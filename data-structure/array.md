# 数组

## 常用方法

### join

拼接数组项为字符串，与之相反的字符串方法split

``` javascript
const arr = [{hello: 'world'},2,3];
let str = arr.join('-'); // '[object Object]-2-3'
```

与join相反的字符串方法`split`，拆分字符串成数组。



### pop

``` javascript
const arr = [{hello: 'world'},2,3];
const res = arr.pop();
console.log(arr); // [ { hello: 'world' }, 2 ]
console.log(res); // 3
```

会改变原数组内容。



### push

``` javascript
const arr = [{hello: 'world'},2,3];
const res = arr.push('hello');
console.log(arr); // [ { hello: 'world' }, 2, 3, 'hello' ]
console.log(res); // 4
```

`push`返回值是数组的长度，会改变原数组内容。



### shift

``` javascript
const arr = [{hello: 'world'},2,3];
const res = arr.shift();
console.log(arr); // [ 2, 3 ]
console.log(res); // { hello: 'world' }
```

会改变原数组内容。



### unshift

``` javascript
const arr = [{hello: 'world'},2,3];
const res = arr.unshift('hello');
console.log(arr); // [ 'hello', { hello: 'world' }, 2, 3 ]
console.log(res); // 4
```

跟`push`一样，新增后返回数组的长度，会改变原数组内容。



### slice

``` javascript
const arr = [{hello: 'world'},2,3];
const res = arr.slice(1);
console.log(arr); // [ { hello: 'world' }, 2, 3 ]
console.log(res); // [ 2, 3 ]

const arr2 = [{hello: 'world'},2,3,4];
const res2 = arr2.slice(1,2);
console.log(arr2); // [ { hello: 'world' }, 2, 3, 4 ]
console.log(res2); // [ 2 ]
```

slice(start,end)截取的是从start到end但不包括end。注意，end可为负数。

不会改变原数组内容，可做浅拷贝。



### splice

``` javascript
const arr = [{hello: 'world'},2,3];
const res = arr.splice(1,1);
console.log(arr); // [ { hello: 'world' }, 3 ]
console.log(res); // [ 2 ]

const arr2 = [{hello: 'world'},2,3,4];
const res2 = arr.slice(1,0,'hello');
console.log(arr2); // [ { hello: 'world' }, 'hello', 2, 3, 4 ]
console.log(res2); // []
```

splice操作后的返回值是裁剪的数组。会更改原数组内容。

### concat

``` javascript
const arr = [{hello: 'world'},2,3];
const res = arr.concat(1);
const res2 = arr.concat([4,5], [6,7]);
console.log(arr); // [ { hello: 'world' }, 2, 3 ]
console.log(res); // [ { hello: 'world' }, 2, 3, 1 ]
console.log(res2); // [ { hello: 'world' }, 2, 3, 4, 5, 6, 7 ]
```

不会更改原数组内容。

### reverse

``` javascript
const arr = [{hello: 'world'},2,3];
const res = arr.reverse();
console.log(arr); // [ 3, 2, { hello: 'world' } ]
console.log(res); // [ 3, 2, { hello: 'world' } ]
```

会更改原数组内容。

### sort

``` javascript
const arr = [{5, 2, 3];
const res = arr.sort();
console.log(arr); // [ 2, 3, 5 ]
console.log(res); // [ 2, 3, 5 ]

const arr2 = [2, 3, 4];
const res2 = arr2.sort((a, b) => {
    return b - a;
});
console.log(arr2); // [ 4, 3, 2 ]
console.log(res2); // [ 4, 3, 2 ]
```

会更改原数组内容。

### indexOf

``` javascript
const arr = [5, 2, 3];
const res = arr.indexOf(2);
console.log(arr); // [ 5, 2, 3 ]
console.log(res); // 1
```



### forEach

``` javascript
const arr = [5, 2, 3];
const res = arr.forEach(i => {
  if(i > 2){
    console.log('hello' + i);
  }
});
// hello5
// hello3
console.log(arr); // [ 5, 2, 3 ]
console.log(res); // undefined
```

forEach没有返回值。

### map

``` javascript
const arr = [5, 2, 3];
const res = arr.map(i => {
  if(i > 2){
    return 'hello' + i ;
  }
  return i;
});
console.log(arr); // [ 5, 2, 3 ]
console.log(res); // [ 'hello5', 2, 'hello3' ]
```

不会改变原数组内容。



### filter

``` javascript
const arr = [5, 2, 3];
const res = arr.filter((i) => i > 2);
console.log(arr); // [ 5, 2, 3 ]
console.log(res); // [ 5, 3 ]
```

筛选出符合条件的数组内容。不改变原数组。

### every

``` javascript
const arr = [5, 2, 3];
const res = arr.every((i) => i < 6);
console.log(arr); // [ 5, 2, 3 ]
console.log(res); // true
```

检测数组中的每一项是否符合条件，返回bool值

### some

``` javascript
const arr = [5, 2, 3];
const res = arr.some((i) => i < 0);
console.log(arr); // [ 5, 2, 3 ]
console.log(res); // false
```

检测数组中的某些项是否符合条件，返回bool值

### find

``` javascript
const arr = [5, 2, 3];
const res = arr.find((i) => i > 2);
console.log(arr); // [ 5, 2, 3 ]
console.log(res); // 5
```

返回数组中符合条件的第一项。若要找多项，用`filter`吧。



### reduce

``` javascript
const arr = [5, 2, 3];
const res = arr.reduce((prev, cur) => prev + cur, 0);
console.log(arr); // [ 5, 2, 3 ]
console.log(res); // 10
```

对数组的每一项从左到右进行累积计算。从右到左使用`reduceRight`。



当然还有一些其他方法，请自行查看。


## 去重
去重的多种方法

## 扁平化
flatten

## 最值
###  排序

``` javascript
const arr = [5, 2, 3];
const res = arr.sort((a, b) => a - b);
const max = arr[arr.length - 1];
const min = arr[0]
console.log(max); // 5
console.log(min); // 2
```



### 假设

``` javascript
const arr = [5, 2, 3];
let max = arr[0];
for (let i = 1; i < arr.length; i++) {
    if (arr[i] > max) {
        max = arr[i];
    }
}
console.log(max); // 5
```

假设第一项是最大的，然后逐个比较。最小值同理。

### Math 

> `Math.max()`函数返回一组数中的最大值  
>
> `Math.min()`函数返回一组数中的最小值

``` javascript
console.log(Math.max(1, 3, 2)); // 3
```

可以看出，`Math.max`接收的是一系列参数。所以我们可以借助`apply`或者扩展运算符`...`来实现

``` javascript
Math.max.apply(null, [1，3，2])；
Math.max(...[1, 3, 2]);
```

`Math.min`同理。

## map实现
数组map的使用
``` javascript
array.map(function(currentValue,index,arr), thisValue)
```
比较需要注意的是map有第二个参数，用于指定回调函数的this指向。
``` javascript
Array.prototype.myMap = function(){
  const newArr = [];
  const [fn, thisArg] = [...arguments];
  for(let i = 0, length = this.length; i < length; i++){
    newArr[i] = fn.call(thisArg, this[i], i, this);
  }
  return newArr;
}
```

``` javascript
// 使用reduce实现
Array.prototype.myMap2 = function(){
  const [fn, thisArg] = [...arguments];
  return this.reduce((prev, cur, index, arr) => {
    prev.push(fn.call(thisArg, cur, index, arr));
    return prev;
  }, [])
}
```



### filter实现

数组filter的使用

``` javascript
array.filter(function(currentValue,index,arr), thisValue)
```

``` javascript
Array.prototype.myFilter = function(){
  const [fn, thisArg] = [...arguments];
  const arr = [];
  for(let i = 0, length = this.length; i < length; i++){
    let meet = fn.call(thisArg, this[i], i, this);
    if(meet){
      arr.push(this[i]);
    }
  }
  return arr;
}
```

``` javascript
Array.prototype.myFilter2 = function(){
  const [fn, thisArg] = [...arguments];
  return this.reduce((prev, cur, index, arr) => {
    let meet = fn.call(thisArg, cur, index, arr);
    if(meet){
      prev.push(cur);
    }
    return prev;
  }, [])
}
```



当掌握了核心思想后，发现实现起来还是挺简单的

## reduce实现

## 扁平数据结构转化为Json数据结构
在树结构章节，对应json数据结构转为扁平数据结构。

## 二维数组--矩阵

