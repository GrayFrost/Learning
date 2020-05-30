# 隐式类型转换
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
| Object    | true                          |

总结：false、undefined、null、""、 +0、-0、NaN为false，其余转成true

tostring

| 参数类型  | 转换结果                                               |
| --------- | ------------------------------------------------------ |
| Undefined | "undefined"                                            |
| Null      | "null"                                                 |
| Boolean   | "true","false"                                         |
| String    | 字符串                                                 |
| Number    | 对应的数字字符串，需要注意的-0转为"0"                  |
| Symbol    | "Symbol(自定义的内容)"                                 |
| Object    | Date对象返回GMT格式字符串,函数对象返回函数代码的字符串 |



```
String(0)  // "0"
String(-Infinity) // "-Infinity"
String(Symbol('hello')) // "Symbol(hello)"
String(true) // "true"
String(NaN) // "NaN"
String(new RegExp()) // "/(?:)/"
```

tonumber

| 参数尅性  | 转换结果 |
| --------- | -------- |
| Undefined |          |
| Null      |          |
| Boolean   |          |
| String    |          |
| Number    |          |
| Symbol    |          |
| Object    |          |



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
