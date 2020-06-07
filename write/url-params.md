# 解析URL Params为对象

``` javascript
var parse = (url) => {
    let reg = /^.+\?(.+)$/;
    const paramsStr = reg.exec(url)[1]; // 取出?后面的数据
    const paramsArr = paramsStr.split("&");
    const urlObj = {};
    paramsArr.forEach((param) => {
        if (/=/.test(param)) {
            let [key, value] = param.split("=");
            value = decodeURIComponent(value);
            value = /\d+/.test(value) ? parseFloat(value) : value;
            if (urlObj.hasOwnProperty(key)) {
                urlObj[key] = [].concat(urlObj[key], value);
            }else{
              urlObj[key] = value
            }
        } else {
            urlObj[param] = true;
        }
    });
    return urlObj;
};
```

``` javascript
let url = 'http://www.domain.com/?user=anonymous&id=123&id=456&city=%E5%8C%97%E4%BA%AC&enabled';
parse(url)
/* 结果
{ user: 'anonymous',
  id: [ 123, 456 ], // 重复出现的 key 要组装成数组，能被转成数字的就转成数字类型
  city: '北京', // 中文需解码
  enabled: true, // 未指定值得 key 约定为 true
}
*/

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

