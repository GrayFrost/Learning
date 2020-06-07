# JSON.stringify和JSON.parse的实现

## JSON.parse
```
let parse = (jsonStr) => {
  return (new Function(`return ${jsonStr}`))()
}
```
通常用new Function的方式，我们就会想到eval也可以实现，但是我看网上的内容中，使用eval来实现，会出现xss问题，需要对参数进行校验处理，记不了一堆正则。