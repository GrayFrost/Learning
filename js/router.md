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
  <div id="routeView"></div>
  <script>
      window.addEventListener("DOMContentLoaded", onLoad);
      window.addEventListener("hashchange", onHashChange);

      var routeView = null;
      function onLoad() {
          routeView = document.querySelector("#routeView");
          onHashChange();
      }

      function onHashChange() {
          switch (location.hash) {
              case "#/home":
                  routeView.innerHTML = "Home";
                  break;
              case "#/about":
                  routeView.innerHTML = "about";
                  break;
              default:
                  routeView.innerHTML = "default";
                  break;
          }
      }
  </script>
</body>
```

``` javascript
class HashRoute {
    constructor() {
        this.routes = {};
        this.currentUrl = "";
    }
    init(){
      window.addEventListener("load", this.update.bind(this));
      window.addEventListener("hashchange", this.update.bind(this));
    }
    route(path, cb) {
        this.routes[path] = cb || function () {};
    }
    update() {
        this.currentUrl = location.hash.slice(1) || "/";
        this.routes[this.currentUrl]();
    }
}
```

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
        this.currentUrl = '';
    }

    init(){
      this.link();
      window.addEventListener('popstate', (e) => {
        this.update(window.location.pathname);
      })
      window.addEventListener('load', () => this.update('/'));
    }

    link(){
      const allLinks = document.querySelectorAll('a[data-href]'); // 自定义的a标签属性
      allLinks.forEach(curLink => {
        curLink.addEventListener('click', e => {
          e.preventDefault();
          const url = curLink.getAttribute('data-href');
          history.pushState({url}, null, url);
        })
      })
    }

    route(path, cb) {
        this.routes[path] = cb || function () {};
    }

    update(url) {
      this.currentUrl = url;
      this.routes[this.currentUrl] && this.routes[this.currentUrl]();
    }
}

```

## 后端路由
url与处理函数的映射
