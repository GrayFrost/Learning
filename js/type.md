# 隐式类型转换
## 数据类型
基本类型
```
Undefined、 Null、 String、 Number、 Boolean、 Symbol (暂不讨论这个
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
