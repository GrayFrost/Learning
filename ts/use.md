# Typescript

## 环境搭建

## 使用
Interface type

相同点

都能描述对象和函数

``` typescript
interface IUser {
  name: string;
  age: number;
}
interface ISetUser {
  (name: string, age: number): void;
}

type User = {
  name: string;
  age: number;
}

type SetUser = (name: string, age: number)=>void;
```

都允许扩展

不同点

type

type除了可声明对象，还能声明基本类型、联合类型、元祖。还可通过typeof获取实例的类型

interface

interface 可以合并声明

``` typescript
interface IUser {
  name: string;
  age: number;
}
interface IUser {
  gender: string;
}
// 最终相当于
interface IUser{
  name: string;
  age: number;
  gender: string;
}
```

存取器

``` typescript
class ITest {
  firstName: string;
  lastName: string;
  constructor(first: string, last: string){
    this.firstName = first;
    this.lastName = last
  }
  get fullName(){
    return `${this.firstName}-${this.lastName}` 
  }
}
```

enum

``` typescript
enum Num {
  ONE,
  TWO = 5,
  THREE
}
console.log(Num.THREE); // 6
```

泛型

断言

declare module

### 第三方库

@types

## lint

