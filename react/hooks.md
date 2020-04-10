# react hooks

## useState
### 使用
`useState`的使用比较简单，不做过多介绍。

### 实现
进行一个简单的模拟useState实现。
我们知道，普通的函数组件是不能维持状态的，当组件刷新时，状态也会更新。然后react的function component在使用了hooks后，即使是组件重新render也能维持旧的状态。这种能够保持旧状态的机制，不正是闭包的特色吗。  
我们首先想到的是如下：

```javascript
import React from 'react';
import ReactDOM from "react-dom";

function myUseState(initState){
  let state = initState;
  function setState(newState){
    state = newState;
    ReactDOM.render(<App />, document.querySelector('#root'));
  }
  return [state, setState]
}

export default function App(){
  const [count, setCount] = myUseState(0);
  return <div>
    <div>
      count:{count}
      <button onClick={() => setCount(count + 1)}>add</button>
    </div>
  </div>;
}
```
当我们点击按钮时，发现毫无反应。其实我们查看整个调用顺序，当我们点击之后，会调用setState的方法，然后重新渲染App组件，然后重新调用自定义的useState，然后内部的state又被我们重新赋值了初始值。所以页面上看起来一直是0。所以我们应该把state提到外部，使其能够进行缓存，然后判断是否有缓存的值，有则不继续使用初始值，而是用上一次的缓存的值，这一步就简单模拟了状态的记录。  
更改如下：

``` diff
+ let state;
function myUseState(initState){
+ state = state || initState;
  function setState(newState){
    state = newState;
    ReactDOM.render(<App />, document.querySelector('#root'));
  }
  return [state, setState]
}
```
以上只是针对一个state的记录，但react hooks的useState是可以针对不同的状态多次使用的。通常维持多状态的数据结构有数组或map等。我们目前先使用数组。  
更改后如下：
``` javascript
import React from 'react';
import ReactDOM from "react-dom";

let memo = [];
let index = 0;
function myUseState(initState){
  let currentIndex = index;
  memo[currentIndex] = memo[currentIndex] || initState;
  
  function setState(newState){
    memo[currentIndex] = newState;
    index = 0
    ReactDOM.render(<App />, document.querySelector('#root'));
  }
  index++;
  return [memo[currentIndex], setState]
}

export default function App(){
  const [count, setCount] = myUseState(0);
  const [status, setStatus] = myUseState(10);
  return <div>
    <div>
      count:{count}
      <button onClick={() => setCount(count + 1)}>add</button>
      status: {status}
      <button onClick={() => setStatus(status + 5)}>add</button>
    </div>
  </div>;
}
```
我们主要关注于`myUseState`部分。在这个组件里，我们使用数组以及index来维护一组数组，这也是react hooks需要在顶层使用的原因，如果在一些条件判断或嵌套里使用，会导致维护状态的index出现混乱。  
我们来理一下上面的逻辑：进入App组件，第一个useState的index是0，添加到数组，此时的setState对应的更新也是更新index为0的数组上的数据（只是还没有触发），然后index加一，为下一个要useState的数据做准备。然后到第二个useState，此时的index已经是1，重复刚才的步骤，然后index加一。我们发现只要调用了setState的方法，页面就会重新刷新，此时需要重置index，方便我们重新查询每一个使用了useState的数据，又因为我们缓存了数据，并且使用了`memo[currentIndex] = memo[currentIndex] || initState`这种判断，使得我们能够拿到上一次的数据。   
当然，react hooks内部使用了链表的方式来维护数据，并且也不是像我们这么简单粗暴的渲染整个入口页面。


### useEffect
闭包陷阱
### useContext
### useReducer
### useCallback
### useMemo
### useRef
### useImperativeHandle
### useLayoutEffect
* 与`useEffect`的区别
### useDebugValue

## 自定义

## 第三方库
### @umi/hooks
查看一些hooks的实现
