# JSON.stringify和JSON.parse的实现

## JSON.stringify


## JSON.parse
``` javascript
let parse = (jsonStr) => {
  return (new Function(`return ${jsonStr}`))()
}
```
通常用`new Function`的方式，我们就会想到`eval`也可以实现，但是我看网上的内容中，使用`eval`来实现，会出现`xss`问题，需要对参数进行校验处理，记不了一堆正则。