# redux
## redux实现
getState
subscribe
dispatch
``` javascript
export function createStore(reducer){
  let currentState = {}
  let observers = [];
  function getState(){
    return currentState
  }
  function dispatch(action){
    currentState = reducer(currentState, action);
    // 更新了数据之后，进行广播
    observers.forEach(fn => fn());
  }
  function subscribe(fn){
    observers.push(fn)
  }
  dispatch({type: 'hahahaha123456'}); // 为了能够初始化state
  return {
    getState,
    dispatch,
    subscribe
  }
}
```

## react-redux
### provider
``` javascript
import React from "react";
export const StoreContext = React.createContext({});

export const Provider = ({ store, children }) => {
    return (
        <StoreContext.Provider value={store}>{children}</StoreContext.Provider>
    );
};

```
### conect
``` javascript
import React from "react";
import { StoreContext } from "./provider";
const { useContext, useEffect, useState } = React;

export function connect(mapStateToProps, mapDispatchToProps) {
    return function wrapConnect(WrapComponent) {
        return function Connect(props) {
          const [update, setUpdate] = useState(0)
            const store = useContext(StoreContext);
            const stateToProps = mapStateToProps(store.getState());
            const dispatchToProps = mapDispatchToProps(store.dispatch);
            useEffect(() => {
              store.subscribe(() => {
                setUpdate(update + 1)
              })
            }, [store, update]);
            return (
                <WrapComponent
                    {...props}
                    {...stateToProps}
                    {...dispatchToProps}
                />
            );
        };
    };
}

```

## redux-thunk

## redux-saga

## 中间件原理
中间件：action与reducer之间
view -> action -> reducer -> store
view -> action -> middleware -> reducer -> store

对dispatch的一个改造

## 中间件实现