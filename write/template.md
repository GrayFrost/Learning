# 模板引擎

``` javascript
const render = (template, data) => {
  let reg = /\{\{(\w+)\}\}/;
  if(reg.test(template)){
    let target = reg.exec(template)[1];
    template = template.replace(reg, data[target]);
    return render(template, data)
  }
  return template;
}
// 我是姓名，年龄18，性别undefined
```

``` javascript
let template = '我是{{name}}，年龄{{age}}，性别{{sex}}';
let data = {
  name: '姓名',
  age: 18
}
```

## 附赠题目
转化为驼峰命名
``` javascript
var s1 = "get-element-by-id"

// 转化为 getElementById
```
``` javascript
const toCamel = (str) => {
  let reg = /-\w/g;
  str = str.replace(reg, function(x) {
    return x.substr(1).toUpperCase();
  })
  return str;
}
```
核心思想是匹配之后，在replace的第二个参数中，使用函数的形式，自定义替换内容。

再转回来
``` javascript
var s2 = 'getElementById';

// 转化为get-element-by-id
```
``` javascript
const toLink = (str) => {
  let reg = /[A-Z]/g
  if(reg.test(str)){
    str = str.replace(reg, function(x) {
      return `-${x.toLowerCase()}`;
    })
  }
  return str;
}
```
其实我是受了模板引擎的启发，一直检测字符串，若有符合格式的内容，替换掉。这也是我在这里加入其它题目的原因。