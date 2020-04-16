# 适配器模式

## 定义
> 适配器模式（Adapter）是将一个类（对象）的接口（方法或属性）转化成适应当前场景的另一个接口（方法或属性）。  

适配，按字面的意思，就是做一层适配转换。我的理解就是将原始数据转成目标预期的格式。

## 例子
我们举个不一样的例子吧。
有一天出现怪兽了，然而身边都是普通人，是无法对抗怪兽的。但我们的主人公小明平时虽然也是普通人，但到危机关头，他就会变身打怪兽。

``` javascript
class Person {
  constructor(name){
    this.name = name;
  }
  show(){
    console.log(`${this.name}的身份是一个普通人`);
  }
}

class Henshi{
  constructor(person){
    this.person = person;
  }
  show(){
    console.log(`${this.person.name}的身份是假面骑士`);
  }
}

const person = new Person('小明');
person.show();// 小明的身份是一个普通人
const rider = new Henshi(person);
rider.show(); // 小明的身份是假面骑士
```
可以看到，我们通过适配器模式，让小明以我们预期的假面骑士身份登场打怪兽了。正义必胜。

再说个简单的例子吧。
在接口定义时，后台经常悄悄的改了接口某个字段。如果我们是直接使用接口的数据字段，并且在多处使用的话，那我们改动起来就很烦。。
``` typescript
// 后台返回数据
interface IBackData {
  a1: string;
}
// 前端指定数据
interface IFrontData {
  a: string;
}

function adaptor(data: IBackData): IFrontData {
  return {
    a: data.a1
  }
}
```
一个常用的方法就是我们对接口返回的数据结构做一层适配，转成我们前端指定的数据结构。那我们只需要改动一个字段，而保证前端项目内部不需要多处调整。不管后台是改成a2还是a3，我们前端使用的都是a。

## 总结
适配器模式可以用于很多地方：组件、方法、数据结构等。我们的例子中举例了数据结构的适配，方法的适配。  
当我们发现我们要用到的数据不太适用于当前环境，而又不想改动原有环境的东西时，我们需要考虑使用适配器模式。简单来说就是兼容。
