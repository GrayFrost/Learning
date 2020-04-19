# 生命周期
新建一个用于测试生命周期的组件
```jsx
import React from 'react';

export default class Life extends React.Component {
  constructor(props){
    super(props);
    console.log('constructor');
    this.state = {count: 0}
  }
  componentWillMount(){
    console.log('componentWillMount')
  }
  componentDidMount(){
    console.log('componentDidMount')
  }
  componentWillUnmount(){
    console.log('componentWillUnmount')
  }
  componentWillReceiveProps(nextProps){
    console.log('componentWillReceiveProps');
  }
  shouldComponentUpdate(nextProps, nextState){
    console.log('shouldComponentUpdate');
    return true;
  }
  componentWillUpdate(nextProps, nextState){
    console.log('componentWillUpdate')
  }
  componentDidUpdate(prevProps, prevState){
    console.log('componentDidUpdate')
  }
  render(){
    console.log('render');
    return <div>life <button onClick={() => {this.setState({
      count: this.state.count+ 1
    })}}>add life state</button></div>
  }
}
```
初始加载时会触发以下生命周期函数
* constructor
* componentWillMount
* render
* componentDidMount

当我们从父组件更改传递给该组件的值时，会触发以下生命周期函数
* componentWillReceiveProps
* shouldComponentUpdate
* componentWillUpdate
* render
* componentDidUpdate

当我们更改该组件内部的状态，即调用setState时，触发以下生命周期函数
* shouldComponentUpdate
* componentWillUpdate
* render
* componentDidUpdate

## 16
```jsx
import React from 'react';

export default class Life extends React.Component {
  constructor(props){
    super(props);
    console.log('constructor');
    this.state = {count: 0}
  }
  componentDidMount(){
    console.log('componentDidMount')
  }
  componentWillUnmount(){
    console.log('componentWillUnmount')
  }
  static getDerivedStateFromProps(nextProps, prevState){
    console.log('getDerivedStateFromProps')
    return {}
  }
  getSnapshotBeforeUpdate(prevProps, prevState){
    console.log('getSnapshotBeforeUpdate');
    return {}
  }
  shouldComponentUpdate(nextProps, nextState){
    console.log('shouldComponentUpdate');
    return true;
  }
  componentDidUpdate(prevProps, prevState){
    console.log('componentDidUpdate')
  }
  render(){
    console.log('render');
    return <div>life <button onClick={() => {this.setState({
      count: this.state.count+ 1
    })}}>add life state</button></div>
  }
}
```
当使用新的生命周期时，需要删除componentWillMount, componentWillReceiveProps, componentWillUpdate。  

初始加载时会触发以下生命周期函数
* constructor
* getDerivedStateFromProps
* render
* componentDidMount

当我们从父组件更改传递给该组件的值时，会触发以下生命周期函数
* getDerivedStateFromProps
* shouldComponentUpdate
* render
* getSnapshotBeforeUpdate
* componentDidUpdate

当我们更改该组件内部的状态，即调用setState时，触发以下生命周期函数
* getDerivedStateFromProps
* shouldComponentUpdate
* render
* getSnapshotBeforeUpdate
* componentDidUpdate

## 常见问题
* 请求放在哪个生命周期？  
请求通常放在`componentDidMount`中。在服务端渲染时，`componentWillMount`会在客户端和服务端各执行一次，所以会触发两次请求。
在16以后，对生命周期做了调整以后，`componentWillMount`可能会触发多次。

* 16新的生命周期 为什么要废弃一些原来的？  
`componentWillMount`的功能完全可以用`constructor`和`componentDidMount`代替。`componentWillReceiveProps`和`componentWillUpdate`在一次更新中可能会出现多次调用的情况，而`componentDidUpdate`在一次更新中只会调用一次。

生命周期的调整，都跟底层的react fiber有关。