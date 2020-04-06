# 策略模式

对于我来说，策略模式是一种可以有效减少代码的模式，最常见的方式是针对大量if-else的抽象。

## 定义

> 定义一系列的算法，把它们一个个封装起来，并且使它们可以相互替换

## 例子

其实很多时候，对于这些理论性的定义还是有点难理解，使用实际的例子反而更容易理解。
常见的概念里会说到，策略模式由两部分构成：一部分为分装不同策略的策略组，另一部分是执行策略的Context。

``` javascript
class Person {
    constructor(type) {
        this.type = type;
    }
    showType() {
        console.log( `我是${this.type}` );
    }
}

class Child extends Person {
    constructor() {
        super('child');
    }
}

class Man extends Person {
    constructor() {
        super('man');
    }
}

class Woman extends Person {
    constructor() {
        super('woman');
    }
}

class Old extends Person {
    constructor() {
        super('old');
    }
}

class WaitingQueue {
    constructor() {
        this.list = [];
    }
    join(type) {
        if (type === 'child') {
            const obj = new Child();
            this.list.push(obj);
        } else if (type === 'man') {
            const obj = new Man();
            this.list.push(obj);
        } else if (type === 'woman') {
            const obj = new Woman();
            this.list.push(obj);
        } else if (type === 'old') {
            const obj = new Old();
            this.list.push(obj);
        }
    }
    show() {
        this.list.forEach(item => {
            item.showType();
        })
    }
}

const list = new WaitingQueue();
list.join('man'); // 一个男士排队
list.join('child'); // 一个小孩排队
list.join('old'); // 一个老人排队
list.join('woman'); // 一个女士排队

list.show();
// 输出如下:
// 我是man
// 我是child
// 我是old
// 我是woman
```

这个例子可能看起来很长，但还是很简单的内容。主要功能是实现一个排队的类，然后显示排队中的人的类型。
我们可以看到我们在WaitingQueue这个类的join方法里进行了大量的判断，那如果有一个新的类型，我们就需要再做一次if-else判断。如果拿设计模式那套来说，join这个方法已经违反了“单一职责”和“开放封闭”了。
首先一个方法里，需要处理男人、女人、小孩等多种逻辑，违反了单一职责。其次，如果他日有一个外星人类（开玩笑），那就需要加上这个判断，违反了开放封闭。

### 重构

回想实现例子之前说的，策略模式需要使用的两部分，我们开始重构。
首先我们需要一个策略组

``` javascript
const strategy = {
    child: Child,
    man: Man,
    woman: Woman,
    old: Old,
}
```

然后我们重新修改join方法的功能

``` javascript
join(type) {
    const obj = new strategy[type]();
    this.list.push(obj);
}
```
现在我们可以看到join的职责就是添加人到排队队列里，而类型的判断已经交给外部的策略来判断

## 总结
用简单的话来说，策略模式的核心就是对象映射。声明一个策略对象，然后通过调用对象里的某个属性（或者说某个策略）来执行对应的方法。
或许我的例子稍微有些不同，但如果多去看看网上所提供的例子，比如表单验证、价格优惠策略等，就会发现使用起来是差不多的，毕竟它们就是同一种模式。

