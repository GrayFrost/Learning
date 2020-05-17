# 节流防抖

## debounce
防抖 key
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
节流 resize scroll
有两种实现方式
一 时间戳
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
二 定时器
``` javascript
```