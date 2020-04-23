# 简单工厂模式

## 定义
> 定义一个工厂类，通过工厂函数，根据传入的参数不同，返回不同的实例。  
简单来说，就是我通过调用一个类，这个类接收特定的参数，然后根据不同的参数执行对应的工厂方法来生成对应的实例。 

## 例子
举个例子，假如把复仇者联盟看成是一个工厂，我们通过指定特定的参数来获得对应的复仇者成员，至于复仇者成员是怎么来的，我们并不关心。
``` javascript
class IconMan {
  constructor(){
    console.log('钢铁侠来了');
  }
}
class SpiderMan {
  constructor(){
    console.log('蜘蛛侠来了');
  }
}
class Hulk {
  constructor(){
    console.log('浩克来了');
  }
}
class Avengers {
  constructor(avenger){
    switch (avenger){
      case 'IconMan': return new IconMan();
      case 'SpiderMan': return new SpiderMan();
      case 'Hulk': return new Hulk();
    }
  }
}

const hero1 = new Avengers('IconMan');
const hero2 = new Avengers('SpiderMan');

// 钢铁侠来了
// 蜘蛛侠来了
```
上面这个例子的工厂类是Avengers，工厂方法是constructor。当然我们在写代码时不一定就在constructor里执行相应判断，可自行调整工厂方法。


## 应用
当我们发现new的操作过于繁琐，希望简化一些流程时，可以考虑用工厂模式进行简单封装。
比如jQuery里中的`$`就用到了工厂模式
``` javascript
class jQuery {
    constructor(selector) {
        super(selector)
    }
    //  ....
}

window.$ = function(selector) {
    return new jQuery(selector)
}
```

查阅网上资料，可以看到jQuery的$、React.createElement等都使用了工厂模式。在学习是发现还有工厂方法模式、抽象工厂模式，暂时未做深入理解。
