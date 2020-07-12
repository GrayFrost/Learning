# 数字千分位

非正则实现：
``` javascript
function formatNumber(str){
  let [int, float] = str.split('.');
  let intArr = int.split('');
  let i = 0;
  let result = []
  while(intArr.length){
    if(i !== 0 && i % 3 === 0){
      result.unshift(intArr.pop()+',')
    }else{
      result.unshift(intArr.pop())
    }
    i++;
  }
  return float ? `${result.join('')}.${float}` : result.join('')
}
let str = '23456789.012';
formatNumber(str); // 23,456,789.012
```

正则实现：
``` javascript
```