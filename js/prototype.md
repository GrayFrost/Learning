# 原型链

首先需要了解几个属性

* `prototype`
* `__proto__`
* `constructor`



`prototype` 函数的原型属性，每个函数都有的显示原型属性。  

`__proto__` 对象所拥有的隐式原型属性。

`constructor`在函数的`prototype`对象中存在的一个属性，用于回指构造函数。

一个基本的概念就是，一个对象的`__proto__`（隐式原型指针）指向生成这个实例的构造函数的`prototype`（原型对象），而函数的原型对象中的`constructor`属性又指回该构造函数。直接举例，简单明了：

``` javascript
function A() {}
let a = new A();
console.log(a.__proto__ === A.prototype); // true
console.log(A.prototype.constructor === A); // true
```



Function.prototype

``` javascript
typeof Function.prototype // function
```



函数对象（区分普通对象）的`__proto__`都指向`Function.prototype`

```
Number.__proto__ === Function.prototype; // true
Boolean.__proto__ === Function.prototype; // true
String.__proto__ === Function.prototype; // true
Object.__proto__ === Function.prototype; // true
Function.__proto__ === Function.prototype; // true
Array.__proto__ === Function.prototype; // true
RegExp.__proto__ === Function.prototype; // true
Error.__proto__ === Function.prototype; // true
Date.__proto__ === Function.prototype; // true
```

所有的函数对象（区别于普通对象）的`__proto__`都指向`Function.prototype`。可以这么理解，凡是可以调用new的对象的隐式指针都指向`Function.prototype`。对这些函数对象使用`typeof`时，得到的则是function。

``` javascript
typeof Number // function
```



而其他一些例如Math、JSON这些直接调用的，它们的`__proto__`则指向`Object.prototype`。

## new

[new做了什么？](../write/new.md)

## instanceof

[instanceof的原理](../write/instanceof.md)

``` javascript
function A() {}
function B() {}

A.prototype = {
  a: 'hello a'
}
const a1 = new A();
const b1 = new B();
console.log(a1 instanceof A); // true
console.log(a1 instanceof Function); // false
console.log(a1 instanceof Object); // true

console.log('a1 a', a1.a); // hello a
A.prototype = {
  noa: 'get out a'
}
const a2 = new A();
console.log(a2 instanceof A); // true
console.log(a1 instanceof A); // false
console.log(a1.a); // hello a
console.log(a2.a); // undefined
console.log(a1.noa); // undefined
console.log(a2.noa); // get out a
```

更改了原型后，instanceof的判断出错，原先通过A构造函数new的实例a，现在竟然不是A的实例了，有点意思。可以当我们重新给prototype赋予一个新的对象时，原先的prototype引用的对象和现在的prototype引用的对象 不同，导致instanceof的原型链判断上判断为这个新的原型对象不在我们这个变量a的原型链上。

而且通过XX.prototype = {}直接重新赋值一个新对象的方式，会导致constructor丢失，注意constructor的重新赋值。

```javascript
function A(){
  this.hello = 'hello'
}
A.prototype.n = 1;
const a1 = new A();
console.log(a1.n); // 1

A.prototype = {
  n:2,
  m:3,
}
const a2 = new A();
console.log(a2.n); // 2
console.log(a2.m); // 3
console.log(A.prototype.constructor === A); // false
```

上面的例子中，需要`A.prototype.constructor = A`。

一般不允许上面这种方式直接更改原型，比如js一些内置的函数对象Array等，如果我们直接使用上述方式，会报错。

```
Array.prototype = {
  sayHello: () => {console.log('hello world')}
}
const arr = new Array();
arr.sayHello(); // TypeError: arr.sayHello is not a function
```

所以我们通常看见的形式都是往原型上添加方法或属性

```
Array.prototype.sayHello = () => {}
```

综上，尽量不要直接重新给原型对象赋值一个新的对象。



### Symbol.hasInstance 

`Symbol.hasInstance`用于判断某对象是否为某构造器的实例。

当我们使用instanceof进行判断时，其实是调用了构造函数内部的Symbol.hasInstance的方法。

比如`arr instanceof Array`相当于`Array[Symbol.hasInstance](arr)`

```javascript
class AlwaysFalse {
  static [Symbol.hasInstance](instance){
    return false;
  }
}
const a = new AlwaysFalse();
console.log(a instanceof AlwaysFalse); // false

// 再举一个例子
class IsMan {
  static [Symbol.hasInstance](instance){
    return instance.gender === 'male';
  }
}
const a = {gender: 'female'};
const b = {gender: 'male'};
console.log(a instanceof IsMan); // false
console.log(b instanceof IsMan); // true
```

就是我们可以自定义instanceof的判断逻辑。

## Object.create()

我们创建对象除了使用new操作符和直接字面的方式外，可以使用`Object.create()`来创建对象。这个方法的第一个参数为新建对象的原型对象，还可以接受第二个参数，但目前我们暂时用不上。

简单模拟,并不能完全实现相同的功能

```javascript
function createObj(proto){
  function F(){}
  F.prototype = proto;
  return new F();
}
```

因为我们直接在控制台中打印下面的，会发现a是一个完完全全的空对象，没有其余多余的属性。

```javascript
const a = Object.create(null); // {}
```

如果要跟平时字面量赋值一样可以

```javascript
const a = Object.create(Object.prototype);
```



看几个例子：

```javascript
const obj1 = {hello: 'a'}
const obj2 = Object.create(obj1);
obj2.hello = 'b';
console.log(obj1); // { hello: 'a' }
console.log(obj2); // { hello: 'b' }
```

``` javascript
const obj1 = {hello: {world: 'a'}}
const obj2 = Object.create(obj1);
obj2.hello.world = 'b';
console.log(obj1); // { hello: { world: 'b' } }
console.log(obj2); // {} 在原型上
```

``` javascript
const obj1 = {hello: ['a', 'a', 'a']}
const obj2 = Object.create(obj1);
obj2.hello[1] = 'b';
console.log(obj1); // { hello: [ 'a', 'b', 'a' ] }
console.log(obj2); // {} 在原型上
```



## Object.getPrototypeOf()

Object.getPrototypeOf，相当于更标准化的`__proto__`

``` javascript
function A(){}
A.prototype = {a: 'hello'}
const a = new A();
console.log(Object.getPrototypeOf(a)); // { a: 'hello' }
```

``` javascript
function A(){}
const a = new A();
console.log(Object.getPrototypeOf(a) === a.__proto__); // true
```



## Object.setPrototypeOf

``` javascript
function A(){}
A.prototype = {a: 'hello'}
const a = new A();

const b = new A();
Object.setPrototypeOf(b, {b: 'world'});
console.log(Object.getPrototypeOf(a)); // { a: 'hello' }
console.log(Object.getPrototypeOf(b)); // { b: 'world' }
console.log(a instanceof A); // true
```

可以发现`setPrototypeOf`更改的是同一个原型对象，跟我们之前重新赋值prototype一个新的对象然后导致instanceof判断不一致是不同的。



## isPrototypeOf

`isPrototypeOf`判断某个对象是否存在于另一个对象的原型链上。

```javascript
function A(){}
A.prototype = {a: 'hello'}
const a = new A();
console.log(A.prototype.isPrototypeOf(a)); // true
```



## hasOwnProperty

`hasOwnProperty`表明对象自身是否具有某个属性，可以检测属性是否是在原型上

``` javascript
function A(b){
  this.b = b;
}
A.prototype= {
  a: 'hello'
}
const a = new A('world');
console.log(a.a); // hello
console.log(a.b); // world
console.log(a.hasOwnProperty('a')); // false
console.log(a.hasOwnProperty('b')); // true
```



## class

类中的方法都在类的prototype上

``` javascript

class A {
  sayHello(){
    console.log('hello world')
  }
}
const a1 = new A();
const a2 = new A();
console.log(a1.hasOwnProperty('sayHello')); // false

function B(){
  this.sayGoodbye = () => {
    console.log('goodbye');
  }
}
const b1 = new B();
console.log(b1.hasOwnProperty('sayGoodbye')) // true
```

可通过Object.assign为类追加方法

``` javascript
class A {
  sayHello(){
    console.log('hello world')
  }
}
const a1 = new A();
console.log(a1.hasOwnProperty('sayHello')); // false
Object.assign(A.prototype, {
  sayHi(){console.log('hi')}
});
a1.sayHi(); // hi
```





## 继承

### 原型继承

构造函数继承

原型链继承

组合继承

组合寄生继承



## 类继承

类继承 class本质上还是构造函数

```javascript
class A{}
console.log(typeof A); // function
```



