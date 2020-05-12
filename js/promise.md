# Promise

## 实现

## 题目

### 按顺序执行promise
``` javascript
const sleep = (ms) =>
    new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve();
        }, ms);
    });

const action1 = () =>  sleep(5000).then(() => {
    console.log('action1')
  })

const action2 = () => sleep(3000).then(() => {
  console.log('action2')
})

const action3 = () => sleep(1000).then(() => {
  console.log('action3')
})

function showAction(actionArr){
  return actionArr.reduce((prev, cur) => {
    return prev.then(cur);
  }, Promise.resolve())
}

function showAction2(actionArr){
  let promise = Promise.resolve();
  actionArr.forEach(action => {
    promise = promise.then(action)
  });
  return promise;
}

showAction([action1, action2, action3]).then(() => {
  console.log('done');
})
```
按顺序执行promise的核心思想就是嵌套promise，然后调用then。简洁的写法就是使用数组的reduce方法。