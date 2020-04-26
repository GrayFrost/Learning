# 原型链

`prototype` 函数 原型属性

`__proto__` 对象 隐式原型属性

`constructor`

new做了啥

`instanceof`

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
console.log(a2.noa); // get out aå
```

更改了原型后，instanceof的判断出错，原先通过Anew的a竟然不是A的实例了，有点意思。

而且通过XX.prototype = {}直接重新赋值一个新对象的方式，会导致constructor丢失，注意constructor的重新赋值

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

上面的例子中，需要`A.prototype.constructor = A`

一般不允许上面这种方式直接更改原型，比如js一些内置的函数对象Array等，如果我们直接使用上述方式，会报错

```javascript
Array.prototype = {
  sayHello: () => {console.log('hello world')}
}
const arr = new Array();
arr.sayHello(); // TypeError: arr.sayHello is not a function
```

所以我们通常看见的形式都是往原型上添加方法或属性

```javascript
Array.prototype.sayHello = () => {}
```





`Object.create()`

我们创建对象除了使用new操作符和直接字面的方式外，可以使用`Object.create()`来创建对象。

简单模拟,并不能完全实现相同的功能

```
function createObj(proto){
  function F(){}
  F.prototype = proto;
  return new F();
}
```

因为我们直接在控制台中打印下面的，会发现a是一个完完全全的空对象，没有其余多余的属性。

```
const a = Object.create(null); // {}
```

如果要跟平时字面量赋值一样可以

```
const a = Object.create(Object.prototype);
```



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
console.log(obj2); // {}
```

``` javascript
const obj1 = {hello: ['a', 'a', 'a']}
const obj2 = Object.create(obj1);
obj2.hello[1] = 'b';
console.log(obj1); // { hello: [ 'a', 'b', 'a' ] }
console.log(obj2); // {}
```

?? 了解一下原理

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



Object.setPrototypeOf

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

可以发现setPrototypeOf更改的是同一个原型对象，跟我们之前重新赋值prototype一个新的对象然后导致instanceof判断不一致是不同的。

isPrototypeOf 某个对象是否存在于另一个对象的原型链上

```javascript
function A(){}
A.prototype = {a: 'hello'}
const a = new A();
console.log(A.prototype.isPrototypeOf(a)); // true
```



hasOwnProperty 表明对象自身是否具有某个属性，可以检测属性是否是在原型上

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



class

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





继承

类继承 class本质上还是构造函数

```javascript
class A{}
console.log(typeof A); // function
```



原型继承

但两种方式还是有区别



构造函数继承

原型链继承

