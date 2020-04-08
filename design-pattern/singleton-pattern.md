# 单例模式
其实在很多场景中，我们都会遇到单例模式。比如vuex/redux的store，

## 定义
> 保证一个类仅有一个实例，并提供一个访问它的全局访问点

## 实现
用两种方式来实现单例模式。一、类的静态方法；二、匿名自执行函数。其实这两种方法大同小异，只是js版本不同的写法。

### ES6
```javascript
class Single {
  constructor(){

  }
  static getInstance(){
    if(!Single.instance){
      Single.instance = new Single();
    }
    return Single.instance;
  }
}
let a = Single.getInstance();
let b = Single.getInstance();
console.log(a === b) // true
```

### ES5
```javascript
function Single() {}
Single.getInstance = (function () {
  let instance;
  return function () {
    if (!instance) {
      instance = new Single();
    }
    return instance;
  };
})();

let a = Single.getInstance();
let b = Single.getInstance();
console.log(a === b); // true
```

## 应用
我们抛弃网上常举比如什么弹窗啊的例子。自己想一个来实现。
比如我们有一个系统，系统里只有一个叫回收站的地方。好，开始。
``` javascript
class GarbageCenter {
  constructor(){
    this.garbages = [];
  }
  static getInstance(){
    if(!GarbageCenter.instance){
      GarbageCenter.instance = new GarbageCenter();
    }
    return GarbageCenter.instance;
  }
  add(item){
    this.garbages.push(item);
  }
  clear(){
    this.garbages = [];
  }
  show(){
    console.log(this.garbages);
  }
}

// 开发人员一调用了回收站功能
const center1 = GarbageCenter.getInstance();
center1.add(1); // 添加了一个垃圾1
// 开发人员二在自己开发的模块也调用了回收站功能
const center2 = GarbageCenter.getInstance();

center2.add(2); // 添加了一个垃圾2
center2.add(3); // 继续添加了一个垃圾3
center1.show(); // [1,2,3]
center2.show(); // [1,2,3]
// 最后两者指向的是同一个回收站，里面都是1,2,3垃圾
```

## 总结
核心思想是使用闭包保存变量，当变量已存在时，不再实例化。