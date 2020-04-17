# 生命周期
新建一个用于测试生命周期的组件
```react
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
```react
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

### 异常处理
* componentDidCatch
通常封装成ErrorBoundary组件。

请求放在哪个生命周期？

16新的生命周期 为什么要废弃一些原来的？