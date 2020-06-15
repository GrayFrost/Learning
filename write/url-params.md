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


