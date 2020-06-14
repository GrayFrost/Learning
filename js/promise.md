# Promise

## 实现
``` javascript
class MyPromise {
    constructor(exector) {
        this.status = "pending";
        this.value = undefined;
        this.reason = undefined;
        this.onResolvedCallbacks = [];
        this.onRejectedCallbacks = [];

        let resolve = (value) => {
            if (this.status === "pending") {
                this.status = "fulfilled";
                this.value = value;
                this.onResolvedCallbacks.forEach((fn) => fn());
            }
        };

        let reject = (reason) => {
            if (this.status === "pending") {
                this.status = "rejected";
                this.reason = reason;
                this.onRejectedCallbacks.forEach((fn) => fn());
            }
        };

        try {
            exector(resolve, reject);
        } catch (e) {
            reject(e);
        }
    }
    then(onFulfilled, onRejected) {
        onFulfilled =
            typeof onFulfilled === "function" ? onFulfilled : (value) => value;
        onRejected =
            typeof onRejected === "function"
                ? onRejected
                : (err) => {
                      throw err;
                  };
        let promise2 = new MyPromise((resolve, reject) => {
            if (this.status === "fulfilled") {
                setTimeout(() => {
                    try {
                        let x = onFulfilled(this.value);
                        resolvePromise(promise2, x, resolve, reject);
                    } catch (e) {
                        reject(e);
                    }
                }, 0);
            }

            if (this.status === "rejected") {
                setTimeout(() => {
                    try {
                        let x = onRejected(this.reason);
                        resolvePromise(promise2, x, resolve, reject);
                    } catch (e) {
                        reject(e);
                    }
                }, 0);
            }
            if (this.status === "pending") {
                this.onResolvedCallbacks.push(() => {
                    setTimeout(() => {
                        try {
                            let x = onFulfilled(this.value);
                            resolvePromise(promise2, x, resolve, reject);
                        } catch (e) {
                            reject(e);
                        }
                    }, 0);
                });
                this.onRejectedCallbacks.push(() => {
                    try {
                        let x = onRejected(this.reason);
                        resolvePromise(promise2, x, resolve, reject);
                    } catch (e) {
                        reject(e);
                    }
                });
            }
        });
        return promise2;
    }
    catch(fn){
      this.then(null, fn)
    }
}

function resolvePromise(promise2, x, resolve, reject) {
    if (promise2 === x) {
        reject("循环引用");
    }
    let called;
    if (x != null && (typeof x === "object" || typeof x === "function")) {
        try {
            let then = x.then;
            if (typeof then === "function") {
                then.call(
                    x,
                    (val) => {
                        if (called) {
                            return;
                        }
                        called = true;
                        resolvePromise(promise2, val, resolve, reject);
                    },
                    (err) => {
                        if (called) {
                            return;
                        }
                        called = true;
                        reject(err);
                    }
                );
            } else {
                resolve(x);
            }
        } catch (e) {
            reject(e);
        }
    } else {
        resolve(x);
    }
}
```
then里promise的返回待处理

### Promise.resolve
``` javascript
MyPromise.resolve = function(val){
  return new MyPromise((resolve, reject) => {
    resolve(val)
  })
}
```

### Promise.reject
``` javascript
MyPromise.reject = function(val){
  return new MyPromise((resolve, reject) => {
    reject(val)
  })
}
```

### Promise.race
``` javascript
MyPromise.race = function(promises){
  return new MyPromise((resolve, reject) => {
    promises.forEach(promise => {
      promise.then(resolve, reject);
    })
  })
  
}
```
### Promise.all
``` javascript
MyPromise.all = function(promises){
  const arr = [];
  let i = 0;
  function handleData(data, index, resolve) {
    arr[index] = data;
    i++;
    if(i === promises.length){
      resolve(arr);
    }
  }
  return new MyPromise((resolve, reject) => {
    promises.forEach((promise, index) => {
      promise.then((data) =>{
        handleData(data, index, resolve)
      }, reject)
    })
  })
}
```
### Promise.finally

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

### 控制并发数