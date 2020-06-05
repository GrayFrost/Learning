# tree-shaking
减少不必要的代码

webpack-deep-scope-plugin

原理

依赖es6模块静态分析

es6 module特点
* 只能作为模块顶层的语句出现
* import的模块名只能是字符串常量
* import binding是immutable的

import 编译时确定模块
require 运行时确定模块，无法分析模块是否可用

有副作用，无法tree-shaking