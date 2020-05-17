# 节流防抖

## debounce
防抖（debounce）：等事件停止执行一段时间不再执行后，才允许继续执行该事件。

``` javascript
function debounce(fn, ms, immediate){
  let timeout;
  return function(){
    let args = [...arguments];
    if(timeout){
      clearTimeout(timeout)
    }
    if(immediate && !timeout){
      fn.apply(this, args);
    }

    timeout = setTimeout(() => {
      fn.apply(this, args);
    }, ms)
  }
}
```

## throttle
节流（throttle）：对于原本是在一段时间内频繁执行的时间，控制为在该段时间内只执行一次。常用语一些高频触发的事件，比如resize、scroll等。

有两种实现方式：
### 时间戳
``` javascript
function throttle(fn, ms){
  let prev = Date.now();
  return function(){
    let now = Date.now();
    let args = [...arguments];
    if(now - prev > ms){
      prev = now;
      fn.apply(this, args);
    }
  }
}
```
### 定时器
``` javascript
function throttle(fn, ms) {
    let timeout = null;
    return function () {
        let args = [...arguments];
        if (!timeout) {
            timeout = setTimeout(() => {
                fn.apply(this, args);
                timeout = null;
            }, ms);
        }
    };
}
```