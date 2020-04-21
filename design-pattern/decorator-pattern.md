# 装饰者模式

## 定义

## 例子
举个例子：有一天出现怪兽了，然而身边都是普通人，是无法对抗怪兽的。但我们的主人公小明平时虽然也是普通人，但到危机关头，他就会变身打怪兽。

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
可以看到，我们通过装饰器模式，让小明以我们预期的假面骑士身份登场打怪兽了，既不影响小明正常的普通人的身份，也能够在特定场合改变身份。正义必胜。

