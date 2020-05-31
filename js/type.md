# 类型转换
## 数据类型
基本类型
```
Undefined、 Null、 String、 Number、 Boolean、 Symbol 
```
复杂类型
```
Object
```

## 类型转换
### 抽象值操作

#### ToString

### ToNumber

### ToBoolean

### ToPrimitive



### 强制类型转换



### 隐式类型转换



js引擎内部转换类型的四个操作。

* ToString
* ToNumber
* ToBoolean
* ToPrimitive

toboolean

| 参数类型  | 转换结果                      |
| --------- | ----------------------------- |
| Undefined | false                         |
| Null      | false                         |
| Boolean   | false为false,true为true       |
| String    | 空字符串""为false,其余为true  |
| Number    | +0,-0，NaN为false，其余为true |
| Symbol    | true                          |

总结：false、undefined、null、""、 +0、-0、NaN为false，其余转成true

tostring

| 参数类型  | 转换结果                              |
| --------- | ------------------------------------- |
| Undefined | "undefined"                           |
| Null      | "null"                                |
| Boolean   | "true","false"                        |
| String    | 字符串                                |
| Number    | 对应的数字字符串，需要注意的-0转为"0" |
| Symbol    | "Symbol(自定义的内容)"                |



```
String(0)  // "0"
String(-Infinity) // "-Infinity"
String(Symbol('hello')) // "Symbol(hello)"
String(true) // "true"
String(NaN) // "NaN"
String(new RegExp()) // "/(?:)/"
```

tonumber

| 参数尅性  | 转换结果                          |
| --------- | --------------------------------- |
| Undefined | NaN                               |
| Null      | 0                                 |
| Boolean   | true为1，false为0                 |
| String    | 符合数字格式的转成数子，其余为NaN |
| Number    | 数字                              |
| Symbol    | 报错                              |



### 原始值到对象的转换



### 对象到原始值得转换

toprimitive

toString

toString() 方法返回一个表示该对象的字符串

valueOf

valueOf() 方法返回指定对象的原始值

| 转成原始类型 | 结果                                                         |
| ------------ | ------------------------------------------------------------ |
| Number       | Date对象返回时间戳<br />空数组为0，数组只有一个元素且该元素符合数字格式转成数字，其余数组内容转成NaN |
| String       | Date对象返回GMT格式字符串<br />函数、class、正则返回源代码的字符串;<br />数组返回join(',')格式的字符串<br />大多数对象转成"[object 类型]"格式 |
| Boolean      | true                                                         |

ToPrimitive ( input [ , PreferredType ] )

PreferredType要么不传，要么是number、string



preferredtype为number

如果input是原始类型，返回

调用valueOf 如果是原始类型，返回

调用toString 如果是原始类型，返回

抛出typeerror



preferredtype为string

如果input是原始类型，返回

调用toString，如果是原始类型，返回

调用valueOf 如果是原始类型，返回

抛出typeerror



preferredtype不传

如果input是date类型，preferredtype为string

否则preferredtype为number

## 隐式转换

通常涉及到隐式转换的运算无非是`+`和`==`。
valueOf toString
[]+[]
{}+{}
[]+{}
{}+[]

==
Object.prototype.toString

 a 在什么情况下会打印 1
```
var a = ?;
if(a == 1 && a == 2 && a == 3){
 	console.log(1);
}
```

```
let a = {
  i: 1,
  valueOf(){
    return this.i++;
  }
}
```
