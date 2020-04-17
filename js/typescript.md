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

Babel7，直接使用`babel-loader`（前提：已安装）。

``` shell
npm install babel-loader @babel/core @babel/preset-env @babel/preset-react -D
```

`babel-loader`的匹配正则修改。

``` javascript
{
    test: /\.(js|jsx|ts|tsx)$/,
    use: "babel-loader",
},
```

`resolve`的`extension`选项添加后缀名`.ts`, `.tsx `。
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

使用了`babel-loader`只是帮我对ts进行了转换，要使项目支持静态类型检测，需配置`tsconfig.json`。

#### 添加tsconfig.json

tsconfig.json配置如下：
``` json
{
  "compilerOptions": {},
  "extends": "",
  "files": [],
  "include": [],
  "exclude": []
}
```
最重要的是`compilerOptions`，其他的可以在有需要时才写。

##### compilerOptions
简单介绍一些配置项。

| 属性名                       | 描述                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| target                       | 将ts代码编译成指定版本的js代码。比如设置成es3，箭头函数则会被编译成正常的function。esnext 因为es每年都有一个版本，设置成这个表示支持最新的es |
| module                       | 模块化方案。我们知道前端模块化有很多种方式。 amd commonjs umd es6等 |
| jsx                          | 后缀名tsx的是使用typescript的jsx                             |
| removeComments               | 编译时删除注释                                               |
| noImplicitAny                | 隐式any，当编译器无法判断类型时，将其判断为any，会报错       |
| strictNullChecks             | 检测null,比如使用数组的find方法可能没有对应项                |
| esModuleInterop              | 我们import React from 'react';发现报错。可以设置这个属性true |
| allowSyntheticDefaultImports | 同上面引用报错，设置allowSyntheticDefaultImports 为true。同时写成import * as React from 'react'; |
| allowJs                      | 比如一个ts文件引用一个js文件时，会报错。设置这个true告诉编译器将这个js文件的方法和变量都设置成any。ts就能识别js了 |



[更多详细配置](https://www.tslang.cn/docs/handbook/compiler-options.html)

## 使用

### interface type

#### 相同点

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

#### 不同点

`type`除了可声明对象，还能声明基本类型、联合类型、元祖。还可通过`typeof`获取实例的类型。

`interface`可以合并声明。

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

### 存取器

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

### enum

``` typescript
enum Num {
  ONE,
  TWO = 5,
  THREE
}
console.log(Num.THREE); // 6
```

### 泛型

泛型是指在定义函数、接口或类的时候，不预先指定具体的类型，使用时再去指定类型的一种特性

最简单的使用：

``` typescript
let arr: Array<number> = [];
```

场景使用：
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

### 断言

``` typescript
interface IObject{
  age: number;
}
let obj = {} as IObject;
obj.age = 18;
```

### declare module

``` typescript
declare module '*.svg'
```

### 高级类型

#### Exclude

``` typescript
// exclude 前后两个类型对比，过滤出前面独有的属性
type T = Exclude<1 | 2, 1 | 3>;
let num: T = 2;
```

#### Extract

``` typescript
// extract 提取出公共的部分
type P = Extract<1 | 2, 1 | 3>;
let num2: P = 1;
```

#### Pick

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

#### Omit

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

### 第三方库
当我们在tyepscript环境的项目里，安装npm包时，通常需要安装对应的@types包。
[@types](https://www.npmjs.com/search?q=%40types)

## Lint

### Tslint
按网上的说法，Typescrpt团队已放弃对tslint的维护，不做过多介绍。

### Eslint

```shell
npm install eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin -D
```

`@typescript-eslint/parser`代替eslint的解析器，使得能够解析ts。

`@typescript-eslint/eslint-plugin`为eslint补充额外的ts校验规则。

在package.json中添加命令

```json
"eslint": "eslint src --ext .ts,.tsx,.js,.jsx"
```
配置`.eslintrc.js`
``` javascript
module.exports = {
    parser: "@typescript-eslint/parser",
    extends: ["plugin:@typescript-eslint/recommended"],
    plugins: ["@typescript-eslint"],
    rules: {
        "no-var": "error",
    },
    parserOptions: {
        ecmaVersion: 6,
        sourceType: 'module',
        ecmaFeatures: {
            jsx: true,
        },
        useJSXTextNode: true,
        project: "./tsconfig.json",
    },
    env: {
        node: true,
        browser: true,
    },
};

```
如果我们不想自定义规则`rules`，我们可以使用一些大厂提供的配置规则，比如airbnb的`eslint-config-airbnb`或腾讯AlloyTeam的`eslint-config-alloy`。 

