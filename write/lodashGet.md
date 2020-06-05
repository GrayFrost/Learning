# 实现lodash的get
## 前情
先看lodash的get方法咋用。
``` javascript
get({ a: null }, 'a.b.c', 3)
// output: 3

get({ a: undefined }, 'a', 3)
// output: 3

get({ a: null }, 'a', 3)
// output: 3

get({ a: [{ b: 1 }]}, 'a[0].b', 3)
// output: 1

```

再由此引申出网上看到的一道题
``` javascript
const obj = { selector: { to: { toutiao: "FE Coder"} }, target: [1, 2, { name: 'byted'}]};

get(obj, 'selector.to.toutiao', 'target[0]', 'target[2].name');
// [ 'FE Coder', 1, 'byted']
```

## 解法
### 正则一
``` javascript
const get = (obj, ...args) => {
  return args.map(arg => {
    const reg = /\[(\d+)\]/gi;
    let target = obj;
    const keys = arg.split('.');
    keys.forEach((key) => {
        try {
            if (reg.test(key)) {
                // 说明是带有[n]的字符串
                const indexStr = key.match(reg)[0]; // 取出[n]
                const keyStr = key.replace(reg, "");
                const index = indexStr.match(/\d+/)[0]; // 取出数字，这里可以有很多取出数字的写法
                target = target[keyStr][index];
            } else {
                target = target[key];
            }
        } catch (e) {
            target = undefined;
        }
    });
    return target;
  })
}
```
核心思想是将字符串拆分成数组，然后对数组中的值，也就是key值进行判断，如果带有`[]`这种数组类型，则区分判断。遍历数组，然后迭代对象的value。

### 正则二
``` javascript
const get4 = (obj, ...args) => {
    return args.map((arg) => {
        const keys = arg.replace(/\[(\d+)\]/, ".$1").split(".");
        let target = obj;
        try {
            keys.forEach((key) => {
                target = target[key];
            });
        } catch (e) {
            target = undefined;
        }
        return target;
    });
};
```
核心思想不变，但是是提前对数组形式做了处理，将类似`a[0]`转成了`a.0`。

### Function
``` javascript
const get = (obj, ...args) => {
    return args.map((arg) => {
        const target = JSON.stringify(obj);
        return new Function(`try{return ${target}.${arg}}catch(e){}`)();
    });
};
```
`new Function()`的方式

### eval
``` javascript
const get = (obj, ...args) => {
    return args.reduce((arr, args) => {
        arr.push(eval(`(function(){try{return obj.${args}}catch(e){}})()`));
        return arr;
    }, []);
};
```
既然`new Function`可以，那`eval`也有异曲同工之妙。这里要注意，如果直接在`eval`里写try的代码，会报错，因为return只能在函数里使用，所以需要写一个自执行函数。