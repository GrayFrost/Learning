# 大数相加

## 大数相加
``` javascript
const addTwoNums = (str1, str2) => {
  let numArr1 = str1.split('').reverse().map(Number);
  let numArr2 = str2.split('').reverse().map(Number);
  let sum = 0, remainder = 0, index = 0, sumArr = [];
  while(numArr1.length || numArr2.length){
    let num1 = numArr1.shift();
    let num2 = numArr2.shift();
    
    sum = (num1 ? num1 : 0) + (num2 ? num2 : 0) + remainder;
    
    sumArr[index] = sum % 10;
    remainder = Math.floor(sum / 10);
    index++;
  }
  if(remainder>0){
    sumArr[index]=remainder;
  }
  return sumArr.reverse().join('');
}
var str1 = '123456789987654321';
var str2 = '9995832109876543210';
console.log(addTwoNums(str1, str2)); // 10119288899864197531
```
做一下LeetCode的第二题，就会发现思路是完全一样的。

## 大数相乘

## 0.1 + 0.2
0.1 + 0.2 !== 0.3
Number.EPSILON