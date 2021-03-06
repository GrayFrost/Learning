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


## useEffect
闭包陷阱
## useContext
在使用useContext之前，我们先看一下React Context的使用。React Context可以帮助我们进行跨层级组件之间的数据传递，可有效避免在每一个层级之间的props手动传递。
``` jsx
const {Provider, Consumer} = React.createContext(defaultValue);

<Provider value={/*共享的数据*/}>
    /*里面可以渲染对应的内容*/
</Provider>

<Consumer>
  {value => /*根据上下文  进行渲染相应内容*/}
</Consumer>
```
首先将父组件用provider包裹，然后子组件或孙子组件用consumer包裹，确保consumer在provider里面。这样当孙子组件需要拿到最顶级组件的状态时，不用通过层层传递了。

举个实际的例子吧
``` jsx
import React from "react";
import ContextDemo from "./use-context";

export const TestContext = React.createContext();
export default function App() {
  return (
    <div className="App">
      <TestContext.Provider
        value={{
          name: "bob"
        }}
      >
        <ContextDemo />
      </TestContext.Provider>
    </div>
  );
}

```
这里直接用子组件来演示其实不太合理，应该用孙子组件的（懒，没办法）。
``` jsx
import React from "react";
import { TestContext } from "./App";
export default function ContextDemo() {
  return (
    <TestContext.Consumer>
      {value => {
        return <div>context value: {value.name}</div>;
      }}
    </TestContext.Consumer>
  );
}

```
那接下来我们看一下使用useContext。
``` jsx
import React from "react";
import { TestContext } from "./App";

export default function ContextDemo() {
  const value = React.useContext(TestContext);
  return <div>context value: {value.name}</div>;
}
```
可以看到，我们不再需要使用consumer来包裹组件，简化了代码。
注意：Provider还是要写的，而且当context值变化时，使用了useContext的组件会重新渲染，需要进行合理的优化。

## useReducer
``` javascript
const [state, dispatch] = useReducer(reducer, initialState);
```
可以配合useContext实现小型的redux状态管理功能。useState的底层代码是依赖于useReducer的，相当于useReducer的语法糖。
用法：
``` javascript
function HooksTest() {
    const [myStore, dispatch] = useReducer(
        (state, action) => {
            switch (action.type) {
                case "add":
                    return {
                        ...state,
                        count: state.count + 1,
                    };
                case "sub":
                    return {
                        ...state,
                        count: state.count - 1,
                    };
                default:
                    return state;
            }
        },
        {
            count: 0,
        }
    );
    return (
        <section>
            <h1>counter: {myStore.count}</h1>
            <button
                onClick={() => {
                    dispatch({ type: "add" });
                }}
            >
                add
            </button>
            <button
                onClick={() => {
                    dispatch({ type: "sub" });
                }}
            >
                sub
            </button>
        </section>
    );
}
```
## useCallback
首先我们来看一个例子：  
``` javascript
import React from "react";
const { useState } = React;
const Child = () => {
    console.log("子组件");
    return <div>子组件</div>;
};

export default function MemoDemo() {
    const [count, setCount] = useState(0);
    return (
        <section>
            <h3>usememo usecallback</h3>
            <label>{count}</label>
            <button onClick={() => setCount(count + 1)}>add</button>
            <Child />
        </section>
    );
}

```
当我们点击按钮更新状态时，发现控制台继续打印了子组件的内容，然而我们这个组件没有用到父组件的状态，我们不希望它更新。我们首先可以使用`React.memo`来缓存。
``` diff
+ const ChildMemo = memo(Child);

export default function MemoDemo() {
    const [count, setCount] = useState(0);
    return (
        <section>
            <h3>usememo usecallback</h3>
            <label>{count}</label>
            <button onClick={() => setCount(count + 1)}>add</button>
+           <ChildMemo />
        </section>
    );
}
```
上面的代码完成后，可以发现更新count，子组件不会重新渲染了。但当我们需要传递props时，使用的memo失效。为啥？
``` javascript
import React, { memo } from "react";
const { useState } = React;
const Child = ({ status, onClick }) => {
    console.log("子组件");
    return (
        <div>
            子组件status{status}
            <button onClick={() => onClick(status + 5)}>add</button>
        </div>
    );
};

const ChildMemo = memo(Child);

export default function MemoDemo() {
    const [count, setCount] = useState(0);
    const [status, setStatus] = useState(10);
    return (
        <section>
            <h3>usememo usecallback</h3>
            <label>{count}</label>
            <button onClick={() => setCount(count + 1)}>add</button>
            <ChildMemo
                status={status}
                onClick={(newStatus) => setStatus(newStatus)}
            />
        </section>
    );
}

```
可以看到，我们传递给子组件的props是status和onClick。但我们点击count更新时，控制台继续打印出子组件的内容。而且当我们不传递onClick给子组件时，会发现子组件并不会更新。可以判断出是这个传递的onClick造成了子组件的更新。因为count更新了，使得count所在的组件进行了更新，而传递给onClick的方法也进行了更新，虽然它还是和原来长的一样，但其实是一个新的函数，导致memo的浅比较也判断它传递onClick这个props是新的，所以我们需要使用到我们的useCallback。

### 使用
``` javascript
useCallback(fn, deps)
```
更新上面的代码：
``` diff
<ChildMemo
    status={status}
+   onClick={useCallback((newStatus) => setStatus(newStatus), [])}
/>
```
因为这个例子里我们不依赖其他参数，在第一次传递之后，后续就不再生成新的函数。那memo经过浅比较之后，发现是同一个函数，就不会重新渲染啦。通常的做法就是后面的依赖项写个空数组，当然如果你的需求里是有一些其依赖则另当别论，只是说明一般的写法。

## useMemo
在上面的例子里，我们传递给子组件的status是一个简单的数字，现在我们改动一下，改成对象。

``` javascript
import React, { memo } from "react";
const { useState, useCallback } = React;
const Child = ({ status, onClick }) => {
    console.log("子组件");
    return (
        <div>
            <label style={{color: status.color}}>子组件status {status.status}</label>
            <button onClick={() => onClick(status.status + 1)}>add</button>
        </div>
    );
};

const ChildMemo = memo(Child);

export default function MemoDemo() {
    const [count, setCount] = useState(0);
    const [status, setStatus] = useState(10);
    return (
        <section>
            <h3>usememo usecallback</h3>
            <label>{count}</label>
            <button onClick={() => setCount(count + 1)}>add</button>
            <ChildMemo
                status={{status, color: status % 2 === 0 ? 'orange': 'pink'}}
                onClick={useCallback((newStatus) => setStatus(newStatus), [])}
            />
        </section>
    );
}

```
发现点击count，传递的status也是一个新的对象，原理就跟上面的callback一样，而我们希望的是只有当status改变的时候，传递的才是一个新的对象。所以使用useMemo。
### 使用
``` javascript
const memoizedValue = useMemo(() => value, deps);
```
注意useMemo也是接收函数和依赖项数组，只是这个函数有返回值。  
更改上面的代码：
``` diff
<ChildMemo
+   status={useMemo(
+       () => ({
+           status,
+           color: status % 2 === 0 ? "orange" : "pink",
+       }),
+       [status]
+   )}
    onClick={useCallback((newStatus) => setStatus(newStatus), [])}
/>
```
只有status更改时，才传递一个新的对象，否则父组件更新时，仍旧使用原来缓存的对象。  
`useMemo`可以用于一些比较复杂的大型计算。我们可以通过`useMemo`指定依赖项，使得组件内有某个不相关的状态更新而导致组件重新渲染时，这部分已经计算的功能是使用缓存的结果，因为我们指定了依赖项。举个例子
``` javascript
export default function MemoDemo() {
    const [count, setCount] = useState(0);
    const [status, setStatus] = useState(10);
    const abc = useMemo(() => {
      // 经过复杂计算后返回值
      return '一个超级复杂的计算后返回的值'
    },[status]);
    return (
        <div />
    );
}
```
当我们更新status以外的state时，整个组件会重新渲染，但复杂的计算不会重新触发，只有当我们更新了status时，才会重新计算。

### 总结
综合上一个小节的useCallback一起总结。很明显useCallback和useMemo都是针对引用类型来说的，函数和对象都是引用类型的值，如果你对一个简单的数字类型变量或字符串变量执行useMemo，那是毫无意义的。这两者的功能都是缓存上一次的引用。 当我们需要传递一个跟父组件状态有关的引用类型数据给子组件或当前组件需要进行一些密集型计算，考虑使用useMemo。当需要把父组件的一个函数传给子组件时，考虑useCallback。为什么说考虑，因为使用这些方法也要开销的，还是要谨慎使用。


## useRef
``` javascript
const refObj = useRef(initialValue);
```
useRef会在每次渲染时返回同一个ref对象。通常可以用来存储一些不想引起页面重新渲染的数据

## useImperativeHandle
控制子组件暴露给父组件的属性，配合forwardRef使用。

## useLayoutEffect
* 与`useEffect`的区别
## useDebugValue
> useDebugValue 用于在 React 开发者工具（如果已安装，在浏览器控制台 React 选项查看）中显示 自定义 Hook 的标签。
> useDebugValue 接受一个格式化函数作为可选的第二个参数。该函数只有在 Hook 被检查时才会被调用。它接受 debug 值作为参数，并且会返回一个格式化的显示值
直接看个例子
``` jsx
import React from "react";
const { useState, useDebugValue } = React;
function useOver(num) {
  const [count, setCount] = useState(0);
  useDebugValue(count > num ? "溢出" : "正常");
  return [count, setCount];
}
export default function DebugDemo() {
  const [count, setCount] = useOver(10);
  return (
    <div>
      count:{count}
      <button
        onClick={() => {
          setCount(count + 2);
        }}
      >
        add
      </button>
    </div>
  );
}
```
我们自定义了一个hooks，主要是在更改count状态时，监测状态是否超出我们设置的值。当我们查看控制台时，可看到
![](../shortcut/react/useDebugValue.png)

## 原理
闭包
链表

## 问题
使用时注意：循环、条件判断、函数嵌套
useEffect闭包陷阱
