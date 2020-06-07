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