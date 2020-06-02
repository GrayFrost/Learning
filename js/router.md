# 路由

## 前端路由
### hash

window.location
window.addEventListenr('hashchange', callback)

### history
history.pushState
history.replaceState
```
history.pushState(stateObj,title,url) or history.replaceState(stateObj,title,url)
```
触发history.back()或浏览器的前进后退按钮，触发popstate事件
window.addEventListener('popstate', callback)
history.pushState和history.replaceState不会触发popstate事件

## 后端路由