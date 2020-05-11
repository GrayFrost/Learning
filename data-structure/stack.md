# 栈

## 实现

实现stack结构相对简单啦

``` javascript
class Stack {
    constructor() {
        this.items = [];
    }
    push(item) {
        this.items.push(item);
    }
    pop() {
        return this.items.pop();
    }
    isEmpty() {
        return this.items.length === 0;
    }
    get size() {
        return this.items.length;
    }
    clear() {
        this.items = [];
    }
}

```



## 使用

直接介绍两道题把

### 进制转换

十进制转二进制。

我们先看一下我们平时自己计算时候的过程。假如有一个十进制数10

* 10 / 2 = 5 余 0
* 5 / 2 = 2 余 1
* 2 / 2 = 1 余 0
* 1 / 2 = 0 余 1

得到每次运算的余数，然后从底往上1010，即得到我们的二进制数

``` javascript
function convert(num){
  const stack = new Stack();
  let reminder;
  while(num > 0){
    reminder = num % 2;
    stack.push(reminder);
    num = Math.floor(num / 2);
  }
  let str = '';
  while(stack.size){
    str += stack.pop();
  }
  return str;
}
```



### 括号匹配

``` javascript
function isMatch(str){
  const map = {
    "(": ")",
    "[": "]",
    "{": "}"
  }
  const stack = new Stack();
  for(let i = 0, length = str.length; i < length; i++){
    let char = str[i];
    if(char in map){
      stack.push(char);
    }else if(map[stack.pop()] !== char){
      return false
    }
  }
  return !stack.size;
}

const str = "([{}]{})"
console.log(isMatch(str)); // true
```

其实这题完全用数组就可以实现了，但我们需要了解其中的栈的思想。