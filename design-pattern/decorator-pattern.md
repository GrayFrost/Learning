# 装饰者模式

## 定义
> 装饰者(decorator)模式能够在不改变对象自身的基础上，在程序运行期间给对像动态的添加职责（方法或属性）

## 例子
直接举个例子。  
``` javascript
class Person{
  constructor(name){
    this.name = name;
    this.skills = [];
  }
  show(){
    console.log(`${this.name}掌握的前端技能`, this.skills);
  }
}

const person1 = new Person('gary');
const person2 = new Person('melantha');
person1.show(); // gary掌握的前端技能 []

class JSDecorator{
  constructor(person){
    this.person = person;
    this.person.skills.push('js');
    return this.person;
  }
}

class CSSDecorator{
  constructor(person){
    this.person = person;
    this.person.skills.push('css');
    return this.person;
  }
}

const person11 = new CSSDecorator(new JSDecorator(person1));
const person22 = new JSDecorator(person2);
person11.show(); // gary掌握的前端技能 [ 'js', 'css' ]
person22.show(); // melantha掌握的前端技能 [ 'js' ]

```
可以看出，当我们不使用调用我们的装饰器的类时，无非打印出我们没有掌握任何技能，并不会影响我们整个流程的操作。当我们使用了装饰器模式后，相当于给我们添加了一些额外的功能，比如我们学会了js，或者学会了css等。

再举个例子：有一天出现怪兽了，然而身边都是普通人，是无法对抗怪兽的。但我们的主人公小明平时虽然也是普通人，但到危机关头，他就会变身打怪兽。

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

## ES7
我们知道ES7有支持装饰器的写法。要支持ES7的语法，还需安装babel插件，并进行配置
``` shell
npm install --save-dev @babel/plugin-proposal-decorators
```
babel配置
``` diff
plugins: [
+   ["@babel/plugin-proposal-decorators", { legacy: true }+ ],
    "@babel/proposal-class-properties",
    "@babel/proposal-object-rest-spread",
],
```
尽量写在最前面。

``` jsx
function classDeco(target){
  target.hasDeco = true;
  return target;
}

@classDeco
class App extends React.Component{
  constructor(props){
    super(props);
  }
  render(){
    return <div />
  }
}
console.log('App 被修饰', App.hasDeco); // App 被修饰 true
```

## 总结
其实在定义的时候，我们就能够联想到高阶函数似乎就跟装饰者模式有很大的联系。

