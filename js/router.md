# 路由

## 前端路由
url与UI之间的映射
在url改变的情况下，不向浏览器发送请求
### hash

window.location
window.addEventListenr('hashchange', callback)

### history
没有了#,当页面刷新时，浏览器仍会发送请求。需要服务器支持，重定向
history.pushState
history.replaceState
```
history.pushState(stateObj,title,url) or history.replaceState(stateObj,title,url)
```
触发history.back()或浏览器的前进后退按钮，触发popstate事件
window.addEventListener('popstate', callback)
history.pushState和history.replaceState不会触发popstate事件

## 后端路由
url与处理函数的映射

## react-router
router context.provider
route context.consumer

link
hash #
history e.preventDefault history.pushState