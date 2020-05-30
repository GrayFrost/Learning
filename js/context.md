# 执行上下文
执行上下文
变量提升
作用域链
this
闭包

## 执行上下文
execution context 
当运行一段代码，js引擎会做一些为变量分配内存，代码上下关联的操作，EC
可以理解为代码解析和执行的环境

执行上下文，即代码的运行环境，分为三种
全局执行上下文 一个程序中只有一个全局执行上下文
函数执行上下文 调用函数时创建，能有多个
eval执行上下文 在eval函数中执行的代码拥有的执行环境

执行上下文栈

执行上下文的生命周期

相关概念
EC: Excution Context
ESC: Excution Context Stack
VO: 变量对象 Variable Object
AO: 活动对象 Active Object
scope chain: 作用域链

变量对象：用来存放执行上下文中可被访问的函数、形参、变量声明等
函数表达式和没有用var声明的变量不会加入
活动对象：AO就是被激活的VO，当进入一个执行上下文时，变量对象被激活

作用域链：规定了如何查找变量，即确定当前执行代码对变量的访问权限。当函数创建时，作用域已确定。

创建阶段：
作用域链
创建变量、函数、形参，该过程详细说明
this
```
executionContext：{
    [variable object | activation object]：{
        arguments,
        variables: [...],
        funcions: [...]
    },
    scope chain: variable object + all parents scopes
    thisValue: context object
}

```

// 新版
this
词法环境
变量环境
```
ExecutionContext = {
  ThisBinding = <this value>,
  LexicalEnvironment = { ... },
  VariableEnvironment = { ... },
}

```


执行阶段：
设置变量的值，函数的引用，解释执行代码

ec相关属性
scope chain
this
VO/AO  vo变量对象   ao活动对象

当一段代码被执行时，会创建执行上下文。执行上下文的创建又分为两个阶段：
1. 创建阶段  当函数被调用，但是开始执行函数内部代码之前

<!-- 创建vo/ao
  过程
创建scope chain -->
词法环境
变量环境
确定上下文中的this指向

2. 执行阶段
设置变量的值，函数的引用，执行代码

闭包
变量提升

作用域链

