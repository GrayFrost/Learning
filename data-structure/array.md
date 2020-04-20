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



### map



### filter



### every



### some



### reduce



当然还有一些其他方法，比如lastIndexOf，reduceRight等，详细信息自行查看。


## 去重
去重的多种方法

## 扁平化
flatten

## 最大最小值
max
min

## map实现

## reduce实现

## 扁平数据结构转化为Json数据结构
在树结构章节，对应json数据结构转为扁平数据结构。



## 二维数组--矩阵

