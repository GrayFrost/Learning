# typescript

## 环境搭建

对于环境，我们分为两种情况。一种是使用脚手架（以create-react-app）为例，另一种是我们自定义项目。

### create-react-app

当我们是使用cra生成一个全新的项目，可以

``` shell
npx create-react-app my-app --typescript
```

当我们是从已有cra创建的react项目中添加typescript时，

``` shell
npm install --save typescript @types/node @types/react @types/react-dom @types/jest
```

添加typescript相关依赖后，我们将项目中的js文件改为 `.tsx` 文件，重启项目，发现根目录下会自动生成 `tsconfig.json` 文件。
可以查看：[create-react-app 添加typescript](https://www.html.cn/create-react-app/docs/adding-typescript/)

### 自定义项目

很多时候，我们可能会自己搭建一个react环境的项目。

#### 安装typescript

``` shell
npm install typescript -D
```

#### 配置webpack

Babel7，直接使用babel-loader（前提：已安装）。

``` shell
npm install babel-loader @babel/core @babel/preset-env @babel/preset-react -D
```

babel-loader的匹配正则修改。

``` javascript
{
    test: /\.(js|jsx|ts|tsx)$/,
    use: "babel-loader",
},
```

resolve的extension选项添加后缀名`.ts`, `.tsx `。
``` javascript
extensions: [".js", ".jsx", ".ts", ".tsx"],
```

然后安装
``` shell
npm install --save-dev @babel/preset-typescript
npm install --save-dev @babel/preset-typescript @babel/preset-env @babel/plugin-proposal-class-properties @babel/plugin-proposal-object-rest-spread
```
再配置`babel.config.js`（前提：新建babel.config.js）
``` javascript
module.exports = {
    presets: [
        "@babel/preset-env",
        "@babel/preset-typescript",
        "@babel/preset-react",
    ],
    plugins: [
        "@babel/proposal-class-properties",
        "@babel/proposal-object-rest-spread",
    ],
};
```

使用了babel-loader只是帮我对ts进行了转换，要使项目支持静态类型检测，需配置tsconfig.json。

#### 添加tsconfig.json

(配置内容)[./tsconfig.md]

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

