# 图片懒加载

原理是监听图片资源所在的容器是否出现在视窗内，以此来决定是否加载图片资源。  
核心思想是判断元素是否处于视口区域。  
整体流程是监听scroll滚动事件，当图片容器出现在视窗时，取出图片上的临时属性data-src（也可以其他data-*）的值赋给src。
优化点是滚动节流，已加载的图片需要过滤，避免重复判断。


client
clientWidth clientHeight 我们设置的宽高
clientLeft clientTop 我们设置的边框值
``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>top</title>
  <style>
    *{
      margin: 0;
      padding: 0;
    }
    #app {
      width: 200px;
      height: 100px;
      border: 10px orange solid;
      position: relative;
      top: 50px;
      left: 150px;
    }
  </style>
</head>
<body>
  <div id="app">hello</div>
  <script>
    let app = document.querySelector('#app');
    console.log('clientWidth', app.clientWidth); // 200
    console.log('clientHeight', app.clientHeight); // 100
    console.log('clientLeft', app.clientLeft); // 10
    console.log('clientTop', app.clientTop); // 10
  </script>
</body>
</html>
```

offset
offsetWidth 设置的宽加上边框
offsetHeight 设置的高加上边框
offsetLeft 获取自身左外边框到定位父级的左内边框的距离，注意定位这个词，是指设置了absolute或fixed的父级元素，没有则继续向上找
offsetTop 获取自身上外边框到定位父级的上内边框的距离

``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>top</title>
  <style>
    *{
      margin: 0;
      padding: 0;
    }
    #app {
      width: 400px;
      height: 400px;
      border: 10px orange solid;
      position: absolute;
      top: 50px;
      left: 150px;
    }
    #content {
      width: 50px;
      height: 50px;
      border: 5px pink solid;
      position: relative;
      top: 20px;
      left: 40px;
    }
  </style>
</head>
<body>
  <div id="app">
    <div id="content"></div>
  </div>
  <script>
    let app = document.querySelector('#app');
    console.log('app clientWidth', app.clientWidth); // 400
    console.log('app clientHeight', app.clientHeight); // 400
    console.log('app clientLeft', app.clientLeft); // 10
    console.log('app clientTop', app.clientTop); // 10

    let content = document.querySelector('#content');
    console.log('content clientWidth', content.clientWidth); // 50
    console.log('content clientHeight', content.clientHeight); // 50
    console.log('content clientLeft', content.clientLeft); // 5
    console.log('content clientTop', content.clientTop); // 5
    console.log('content offsetWidth', content.offsetWidth); // 60
    console.log('content offsetHeight', content.offsetHeight); // 60
    console.log('content offsetLeft', content.offsetLeft); // 40
    console.log('content offsetTop', content.offsetTop); // 20
  </script>
</body>
</html>
```

scroll
scrollWidth
scrollHeight
scrollLeft
scrollTop

``` html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>top</title>
        <style>
            * {
                margin: 0;
                padding: 0;
            }
            #app {
                width: 400px;
                height: 400px;
                border: 10px orange solid;
                position: absolute;
                top: 50px;
                left: 150px;
            }
            #content {
                width: 200px;
                height: 200px;
                border: 5px pink solid;
                position: relative;
                top: 20px;
                left: 40px;
                overflow: auto;
            }
            #inner {
                width: 250px;
                height: 250px;
                background-color: purple;
            }
        </style>
    </head>
    <body>
        <div id="app">
            <div id="content">
                <div id="inner"></div>
            </div>
        </div>
        <script>
            let app = document.querySelector("#app");
            console.log("app clientWidth", app.clientWidth); // 400
            console.log("app clientHeight", app.clientHeight); // 400
            console.log("app clientLeft", app.clientLeft); // 10
            console.log("app clientTop", app.clientTop); // 10
            console.log("app offsetWidth", app.offsetWidth); // 420
            console.log("app offsetHeight", app.offsetHeight); // 420
            console.log("app offsetLeft", app.offsetLeft); // 150
            console.log("app offsetTop", app.offsetTop); // 50
            console.log("app scrollWidth", app.scrollWidth); // 400
            console.log("app scrollHeight", app.scrollHeight); // 400
            console.log("app scrollLeft", app.scrollLeft); // 0
            console.log("app scrollTop", app.scrollTop); // 0

            let content = document.querySelector("#content");
            console.log("content clientWidth", content.clientWidth); // 200
            console.log("content clientHeight", content.clientHeight); // 200
            console.log("content clientLeft", content.clientLeft); // 5
            console.log("content clientTop", content.clientTop); // 5
            console.log("content offsetWidth", content.offsetWidth); // 210
            console.log("content offsetHeight", content.offsetHeight); // 210
            console.log("content offsetLeft", content.offsetLeft); // 40
            console.log("content offsetTop", content.offsetTop); // 20
            console.log("content scrollWidth", content.scrollWidth); // 250
            console.log("content scrollHeight", content.scrollHeight); // 250
            content.onscroll = function () {
                console.log("content scrollLeft", this.scrollLeft); // 动态变化
                console.log("content scrollTop", this.scrollTop); // 动态变化
            };
        </script>
    </body>
</html>

```

scroll [节流](../performance/debounce.md)

getboundingclientrect

intersection observer