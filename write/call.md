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
Object.prototype.toString
Math.max
slice

### 实现

## apply
### api
``` javascript
fun.apply(thisArg, [param1,param2,...])
```
apply与call的差异点主要在于后面传递的参数的格式，是一个数组类型。所以不再重复介绍使用

### 实现

## bind
### api
```javascript
fun.bind(thisArg, param1, param2, ...)
```

### 实现