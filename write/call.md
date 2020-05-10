# call、apply、bind

this的显式绑定的三种方法：call、apply、bind。
call、apply是立即调用，返回函数执行的结果，bind返回的是一个函数。
常见场景就是借用方法，减少重复代码的编写。你都已经有这个方法了，就借我用一下吧。

## call

### api
``` javascript
fun.call(thisArg, param1, param2, ...)
```
thisArg:fun函数运行时this的指向，可能的值为：
* 不传，或者传null，undefined，this指向window对象
* 另一个函数，this指向该函数引用
* 原始值（字符串、数字、布尔值），this指向该原始值的自动包装对象（String、Number、Boolean）
* 一个对象，this指向该对象

分别举例
``` javascript
var name = 'global';
function hello(){
  console.log(this.name)
}
hello.call(); // global
```
说实话，其实这种写法更像是标准的写法，就是某某函数被调用了，然后再指明其被调用时的执行上下文。

``` javascript
var name = 'global';
function hello(){
  console.log(this.name)
}

function world(){}
hello.call(world); // world 
// 应为函数world的name的值就是函数名world
```

``` javascript
var name = 'global';
function hello(){
  console.log(this.name)
}

var str = 'str';
hello.call(str); // undefined
```

``` javascript
var name = 'global';
function hello(){
  console.log(this.name)
}

var obj = {
  name: 'local'
}
hello.call(obj); // local
```

后面的param表示需要传递的参数，可选。

### 使用

#### slice

我们经常可以看到获取函数参数并转成数组的方式。

``` javascript
function foo(){
  const args = [].slice.call(arguments);
  console.log(args);
}

foo(1,2,3); // [1,2,3]
```

为什么能将类数组转成数组呢。我们假定slice的内容实现是这样的，生成一个新数组，然后for遍历this,并把this[i]赋给新数组，再返回新数组。我们调用了slice方法，它的执行上下文中的this指向类数组。类数组是可以使用for遍历的。



#### toString

经典的判断变量类型的方法Object.prototype.toString，首先我们看一下该方法的作用

``` javascript
let obj = {
  foo: 'bar',
}

console.log(obj.toString()); // [object Object]
```

然后再看一些其他类型的toString方法

```javascript
let num = 1;

console.log(num.toString()); // "1"
```

``` javascript
let str = 'foo';

console.log(str.toString()); // "foo"
```

``` javascript
let bool = true;

console.log(bool.toString()); // "true"
```

``` javascript
function foo(){}

console.log(foo.toString()); // function foo(){} 
```

``` javascript
let arr = [1,2,3]

console.log(arr.toString()); // 1,2,3
```

可以看到不同类型变量调用toString()返回的值得形式也是不同。我们假定Object.prototype.toString的内部实现是判断this的类型是什么，然后返回类似`[object 类型]`的形式。而且我们之前在说传递参数的时候说明过了，如果this传递的是原始值，它会自动包装成对象。我们调用Object.prototype.toString方法，然后指定this,它就会判断this的类型是什么，然后返回给我们一个字符串。

``` javascript
let num = 1;
let str = 'foo';
let bool = true;
let func = () => {}
let arr = [1,2,3];

console.log(Object.prototype.toString.call(num)); // [object Number]
console.log(Object.prototype.toString.call(str)); // [object String]
console.log(Object.prototype.toString.call(bool)); // [object Boolean]
console.log(Object.prototype.toString.call(func)); // [object Function]
console.log(Object.prototype.toString.call(arr)); // [object Array]
```



### 实现

call的原理可以简单理解为

```javascript
var obj = {
    value: 'hello'
}

function show() {
    console.log(this.value)
}
show.call(obj) // hello
```

转变成如下格式

``` javascript
var obj = {
    value: 'hello',
    show: function() {
        console.log(this.value)
    }
}
```

就是要在obj对象里添加一个方法，这个方法就是我们外层要执行的方法，然后调用这个方法。我们自己添加了一个方法，最后执行完肯定要删掉它，保持原来的对象不变。

真正的实现来了

``` javascript
Function.prototype.myCall = function(context) { // 这里有个注意的点是不要用箭头函数，不然拿不到this啦
  let ctx = context || 'window';
  let fn = Symbol();
  ctx[fn] = this; // 避免与执行对象上的属性方法冲突
  let args = [...arguments].slice(1); // 直接使用es6的方法了，es3的eval太麻烦
  let result = ctx[fn](...args);
  delete ctx[fn];
  return result;
}
```

```javascript
function foo(count){
  console.log(this.age + count)
}

let obj = {
  age: 12
}

foo.myCall(obj, 8); // 20
```



## apply
### api
``` javascript
fun.apply(thisArg, [param1,param2,...])
```
apply与call的差异点主要在于后面传递的参数的格式，是一个数组类型。所以不再重复介绍使用



### 使用

#### max

Math.max()返回一组数中的最大值，它接收的是多个参数。如果我们要直接判断数组中的最大值呢。

``` javascript
const max = Math.max.apply({foo: 'bar'}, [1,2,3]);
console.log(max); // 3
```

在网上的例子可以看到Math.max.apply(null, arr)或Math.max.apply(Math, arr)的形式，但我传了一个对象同样是可行的，说明Math.max内容比没有用到执行上下文的this的东西，我们传啥都行，这里调用apply主要是更改传递参数的形式。

### 实现

``` javascript
Function.prototype.myCall = function(context) {
  let ctx = context || 'window';
  let fn = Symbol();
  ctx[fn] = this;
  let args = [...arguments].slice(1);
  let result
  if(args){
    result = ctx[fn](...args[0]);
  }else{
    result =ctx[fn]();
  }
  delete ctx[fn];
  return result;
}
```



## bind
### api
```javascript
fun.bind(thisArg, param1, param2, ...)
```

使用bind返回的是一个待调用的函数

### 使用

```javascript
function foo(num1, num2, num3){
  console.log(this.text);
  console.log(num1 + num2+ num3);
}

const obj = {
  text: 'obj'
}

let fn = foo.bind(obj, 1);
fn(2, 3);
// obj
// 6
```

首先bind接收的第一个参数是指定的this，之后接收的是调用函数的参数列表，然后返回一个待调用的函数。调用返回的函数时，仍然可以继续传递参数，bind会将这些参数收集后统一处理。



bind返回的函数是能够作为构造函数调用的，此时原先绑定的this值会失效，但传递的参数有效。

```javascript
function foo(gender, num1, num2, num3){
  this.gender = gender;
  this.age = num1 + num2 + num3;
  console.log(this);
  console.log(this.text);
}

foo.prototype.age = 18;

const obj = {
  text: 'obj'
}

let fn = foo.bind(obj, 'male');
const a = new fn(1,2,3);
console.log(a.age);
console.log(a.gender);
// foo { gender: 'male', age: 6 }
// undefined
// 6
// male
```

可以发现，使用了new之后，this指向了构造函数生成的实例。



### 实现

实现结合使用一起看，效果更佳。

先实现一版简单的

``` javascript
Function.prototype.myBind = function(context){
  let ctx = context || 'window';
  let arg1 = [...arguments].slice(1);
  let fn = Symbol();
  ctx[fn] = this;
  return function(){ // 返回一个待调用的函数
    let arg2 = [...arguments]; // 继续接收参数
    let result = ctx[fn](...[...arg1, ...arg2]); // 统一收集到函数需要的参数
    delete ctx[fn]
    return result;
  }
}
```

因为知道了bind返回的函数可以作为构造函数使用，我们首先需要对第一版代码进行改动。

在返回的函数内部需要判断this当前是否是属于构造函数的实例，如果是，说明使用了new，否则重新把函数的执行上下文绑定回bind最开始传递的参数。

在上面的使用例子中，我们发现使用new生成的实例，是可以获取到原型上的属性的，所以我们还需要为我们返回的函数绑定原型。

``` javascript
Function.prototype.myBind = function(context){
  let ctx = context || 'window';
  let arg1 = [...arguments].slice(1);
  let self = this;
  function F(){
    let arg2 = [...arguments];
    let judgeThis = this instanceof F ? this : ctx;
    self.apply(judgeThis, [...arg1, ...arg2]);
  }
  F.prototype = this.prototype;
  return F;
}
```

