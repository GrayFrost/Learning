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

js引擎内部转换类型的四个操作。

* ToString
* ToNumber
* ToBoolean
* ToPrimitive

#### ToString

| 参数类型  | 转换结果                              |
| --------- | ------------------------------------- |
| Undefined | "undefined"                           |
| Null      | "null"                                |
| Boolean   | "true","false"                        |
| String    | 字符串                                |
| Number    | 对应的数字字符串，需要注意的-0转为"0" |
| Symbol    | "Symbol(自定义的内容)"                |



#### ToNumber

| 参数尅性  | 转换结果                          |
| --------- | --------------------------------- |
| Undefined | NaN                               |
| Null      | 0                                 |
| Boolean   | true为1，false为0                 |
| String    | 符合数字格式的转成数子，其余为NaN |
| Number    | 数字                              |
| Symbol    | 报错                              |



#### ToBoolean

| 参数类型  | 转换结果                      |
| --------- | ----------------------------- |
| Undefined | false                         |
| Null      | false                         |
| Boolean   | false为false,true为true       |
| String    | 空字符串""为false,其余为true  |
| Number    | +0,-0，NaN为false，其余为true |
| Symbol    | true                          |

总结：false、undefined、null、""、 +0、-0、NaN为false，其余转成true

#### ToPrimitive

toprimitive

toString

toString() 方法返回一个表示该对象的字符串

| Number   | 调用toString                                                 |
| -------- | ------------------------------------------------------------ |
| Number   | 返回数字的字符串形式                                         |
| String   | 返回原字符串                                                 |
| Boolean  | 'true'或'false'                                              |
| Object   | 返回'[object 类型名]'                                        |
| Array    | 将数组元素转换为字符串，用逗号拼接并返回。相当于调用了join(',') |
| Function | 返回函数的源代码字符串                                       |
| Date     | 返回GMT格式字符串                                            |
| RegExp   | 返回文本格式为'/pattern/flag'                                |

valueOf

valueOf() 方法返回指定对象的原始值

| Number   | 返回原始类型的数字值                     |
| -------- | ---------------------------------------- |
| Number   | 返回原始类型的数字值                     |
| String   | 返回原始类型的字符串值                   |
| Boolean  | 返回原始类型的布尔值                     |
| Object   | 返回对象本身                             |
| Array    | 方法继承于Object.prototype，返回原数组   |
| Function | 方法继承于Object.prototype，返回函数本身 |
| Date     | 返回时间戳                               |
| RegExp   | 方法继承于Object.prototype，返回值本身   |



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



### 强制类型转换

String()

Number()

Boolean()

### 隐式类型转换

#### 加法

#### 比较

1. undefined == null，结果为true,undefined或null与其他值比较都是false
2. String == Boolean,都转成Number比较
3. String/Boolean == Number，需要将String/Boolean转成Number
4. Object == Primitive，需要Object转为Primitive







| 转成原始类型 | 结果                                                         |
| ------------ | ------------------------------------------------------------ |
| Number       | Date对象返回时间戳<br />空数组为0，数组只有一个元素且该元素符合数字格式转成数字，其余数组内容转成NaN |
| String       | Date对象返回GMT格式字符串<br />函数、class、正则返回源代码的字符串;<br />数组返回join(',')格式的字符串<br />大多数对象转成"[object 类型]"格式 |
| Boolean      | true                                                         |



## 题目

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
