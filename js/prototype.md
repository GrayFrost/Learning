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

更改了原型后，instanceof的判断出错，原先通过Anew的a竟然不是A的实例了，有点意思



Object.create

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

Object.getPrototypeOf

``` javascript
function A(){}
A.prototype = {a: 'hello'}
const a = new A();
console.log(Object.getPrototypeOf(a)); // { a: 'hello' }
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

isPrototypeOf 某个对象是否是另一个对象的原型

```javascript
function A(){}
A.prototype = {a: 'hello'}
const a = new A();
console.log(A.prototype.isPrototypeOf(a)); // true
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



继承

类继承 class本质上还是构造函数

```
class A{}
console.log(typeof A);
```



原型继承

但两种方式还是有区别



构造函数继承

原型链继承

