# typescript

## 环境搭建

### create-react-app

[create-react-app 添加typescript](https://www.html.cn/create-react-app/docs/adding-typescript/)

按照步骤，添加typescript相关依赖后，将js改成tsx。重启项目，发现根目录下会自动生成 `tsconfig.json` 文件，内容如下：

``` json
{
  "compilerOptions": {
    "target": "es5",
    "lib": [
      "dom",
      "dom.iterable",
      "esnext"
    ],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react"
  },
  "include": [
    "src"
  ]
}

```

### 自定义项目

#### 安装typescript

``` shell
npm install typescript -D
```

#### 配置webpack

Babel7，直接使用babel-loader。babel-loader的匹配正则修改；resolve extension 添加后缀名.ts .tsx 

使用了babel-loader只是帮我对ts进行了转换，要使项目支持静态类型检测，需配置tsconfig.json

#### 添加tsconfig.json

了解配置内容

##### target

target 将ts代码编译成指定版本的js代码

比如设置成es3，箭头函数则会被编译成正常的function

esnext 因为es每年都有一个版本，设置成这个表示支持最新的es

##### module

module 模块。我们知道前端模块化有很多种方式。 amd commonjs umd es6等

##### jsx

后缀名tsx的是使用typescript的jsx

##### removeComments

removeComments 编译时删除注释

##### noImplicitAny

隐式any，当编译器无法判断类型时，将其判断为any，会报错

##### strictNullChecks

检测null, 比如使用数组的find方法可能没有对应项

##### esModuleInterop

我们import React from 'react'; 发现报错

设置这个属性true

##### allowSyntheticDefaultImports

同上面引用报错，设置allowSyntheticDefaultImports 为true

同时写成import * as React from 'react'; 

##### allowJs 

比如一个ts文件引用一个js文件时，会报错。设置这个true告诉编译器将这个js文件的方法和变量都设置成any。ts就能识别js了

[更多详细配置](https://www.tslang.cn/docs/handbook/compiler-options.html)

## 使用

#### interface type

##### 相同点

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

##### 不同点

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

#### 存取器

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

#### enum

``` typescript
enum Num {
  ONE,
  TWO = 5,
  THREE
}
console.log(Num.THREE); // 6
```

#### 泛型

泛型是指在定义函数、接口或类的时候，不预先指定具体的类型，使用时再去指定类型的一种特性

最简单的使用

``` 
let arr: Array<number> = [];
```

``` typescript
interface IListItem {
    name: string;
}

interface IListItem2 {
    age: number;
}

async function getList(): Promise<IRes<IListItem>> {
    const res = await Promise.resolve([{ name: "hello" }]);
    return {
        status: "success",
        total: 1,
        list: res,
    };
}

async function getList2(): Promise<IRes<IListItem2>> {
    const res = await Promise.resolve([{ age: 18 }]);
    return {
        status: "error",
        total: 1,
        list: res,
    };
}
```

#### 断言

``` typescript
interface IObject{
  age: number;
}
let obj = {} as IObject;
obj.age = 18;
```

#### declare module

``` typescript
declare module '*.svg'
```

#### 高级类型

##### Exclude

``` typescript
// exclude 前后两个类型对比，过滤出前面独有的属性
type T = Exclude<1 | 2, 1 | 3>;
let num: T = 2;
```

##### Extract

``` typescript
// extract 提取出公共的部分
type P = Extract<1 | 2, 1 | 3>;
let num2: P = 1;
```

##### Pick

``` typescript
type Person ={
  id: string;
  name: string;
  age: number;
}

// 选出需要的
const person1: Pick<Person, 'name' | 'age'> = {
  name: 's',
  age: 18
}
```

##### Omit

``` typescript
type Person ={
  id: string;
  name: string;
  age: number;
}

// 选出不需要的
const person2: Omit<Person, 'id'> = {
  name: 'ss',
  age: 20
}
```

#### 第三方库

@types

## Lint

#### Tslint

#### Eslint

