# tsconfig.json
tsconfig.json配置

target 将ts代码编译成指定版本的js代码

比如设置成es3，箭头函数则会被编译成正常的function

esnext 因为es每年都有一个版本，设置成这个表示支持最新的es



module 模块。我们知道前端模块化有很多种方式。 amd commonjs umd es6等



jsx

后缀名tsx的是使用typescript的jsx



removeComments 编译时删除注释



noImplicitAny 隐式any，当编译器无法判断类型时，将其判断为any，会报错



strictNullChecks 检测null,比如使用数组的find方法可能没有对应项



esModuleInterop

我们import React from 'react';发现报错

设置这个属性true



allowSyntheticDefaultImports

同上面引用报错，设置allowSyntheticDefaultImports 为true

同时写成import * as React from 'react';



allowJs 比如一个ts文件引用一个js文件时，会报错。设置这个true告诉编译器将这个js文件的方法和变量都设置成any。ts就能识别js了

