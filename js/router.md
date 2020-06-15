# 路由

## 前端路由
url与UI之间的映射
在url改变的情况下，不向浏览器发送请求
### hash

window.location
window.addEventListenr('hashchange', callback)

``` html
<body>
  <ul>
      <li><a href="#/home">home</a></li>
      <li><a href="#/about">about</a></li>
  </ul>
  <div id="content"></div>
</body>
```

``` javascript
class HashRoute{
  constructor(){
    this.routes = {}
    window.addEventListener('load', this.update.bind(this));
    window.addEventListener('hashchange', this.update.bind(this))
  }
  route(path, cb){
    this.routes[`#${path}`] = cb;
  }
  update(){
    let current = location.hash;
    this.routes[current] && this.routes[current]();
  }
}
```

### history
没有了#,当页面刷新时，浏览器仍会发送请求。需要服务器支持，重定向。
history.pushState
history.replaceState
```
history.pushState(stateObj,title,url) or history.replaceState(stateObj,title,url)
```
触发history.back()或浏览器的前进后退按钮，触发popstate事件
window.addEventListener('popstate', callback)
`history.pushState`和`history.replaceState`不会触发popstate事件。只有用户点击浏览器倒退按钮和前进按钮，或者使用 JavaScript 调用back、forward、go方法

``` html
<body>
  <ul>
      <li><a href="/home">home</a></li>
      <li><a href="/about">about</a></li>
  </ul>
  <div id="routeView"></div>
  <script>
      window.addEventListener("DOMContentLoaded", onLoad);
      window.addEventListener("popstate", onPopState);
      var routeView = null;
      function onLoad() {
          routeView = document.querySelector("#routeView");
          onPopState();

          var linkList = document.querySelectorAll("a[href]");
          linkList.forEach((el) =>
              el.addEventListener("click", function (e) {
                  e.preventDefault();
                  history.pushState(null, "", el.getAttribute("href"));
                  onPopState();
              })
          );
      }
      function onPopState() {
          switch (location.pathname) {
              case "/home":
                  routeView.innerHTML = "home";
                  break;
              case "/about":
                  routeView.innerHTML = "about";
                  break;
              default:
                  routeView.innerHTML = "home";
                  break;
          }
      }
  </script>
</body>
```

``` javascript
class HistoryRoute {
    constructor() {
        this.routes = {};
        this.modifyLinks();
        window.addEventListener("load", this.update.bind(this));
        window.addEventListener("popstate", (e) => {
            console.log("zzh post state", e);
            this.update(e.state.path);
        });
    }
    route(path, cb) {
        this.routes[path] = cb;
    }
    modifyLinks() {
        const allLinks = document.querySelectorAll("a[data-href]");
        allLinks.forEach((link) => {
            link.addEventListener("click", (e) => {
                e.preventDefault();
                let current = e.target.getAttribute("data-href");
                console.log("zzh current", current);
                history.pushState({ path: current }, null, current);
            });
        });
    }
    update(path) {
        this.routes[path] && this.routes[path]();
    }
}

```

## 后端路由
url与处理函数的映射
