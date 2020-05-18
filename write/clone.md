# 拷贝
深拷贝和浅拷贝都是针对引用类型而言。拷贝就是开辟一个新的内存空间，存储一份旧数据的复制，使得这两份数据不再指向同一个存储地址。所谓的浅和深，是浅拷贝只会拷贝一层，深拷贝则层层拷贝。基于这句话，我们可以先简单的写出两种方法的代码

``` javascript
// 浅拷贝
function shallowClone(source) {
    let target = {};
    for (let key in source) {
        target[key] = source[key];
    }
    return target;
}
```

``` javascript
// 深拷贝
function deepClone(source) {
    let target = {};
    for (let key in source) {
        target[key] =
            typeof source[key] === "object"
                ? deepClone(source[key])
                : source[key];
    }
    return target;
}
```

## 浅拷贝
* `Array.prototype.slice()`
* `Array.prototype.concat()`
* `Array.from()`
* `Object.assign()`

使用浅拷贝的方法，当子属性是引用类型时，修改子属性的值会影响原有数据。

## 深拷贝

### 序列化
``` javascript
JSON.parse(JSON.stringify())
```
这种方式无法处理属性中的`undefined`、`Symbol`、`函数`。

### 优化
现在需要对最开始写的深拷贝进行完善。
* 判断传入的参数是否是引用类型
* 引用类型数据有对象和数组，需要区分
* 处理循环引用的问题
``` javascript
function deepClone(source, hash = new WeakMap()) {
    if (!isObject(source)) {
        return source;
    }
    if (hash.has(source)) {
        return hash.get(source);
    }
    let target = Array.isArray(source) ? [] : {};
    hash.set(source, target);
    for (let key in source) {
        target[key] = isObject(source[key])
            ? deepClone(source[key], hash)
            : source[key];
    }
    return target;
}

function isObject(obj) {
    return Object.prototype.toString.call(obj) === "[object Object]";
}
```
当然还有其他一些优化点：
* 递归爆栈
* Symbol处理

### 深度优先

### 广度优先