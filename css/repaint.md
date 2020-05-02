# 重绘与回流

## 浏览器渲染机制
html解析成DOM，css解析成CSSOM，将DOM和CSSOM结合，得到渲染书（render tree），根据render tree，计算所有节点的大小和位置，最后渲染到页面上。

<b>回流一定会引起重绘，重绘不一定会引起回流。</b>

## 回流（Reflow）
render tree 的部分或全部元素发生尺寸、布局的改变时，会发生回流。回流就是在渲染树计算节点在视窗的大小和位置的过程。

会导致回流的一些操作：
* 页面初始化（所以项目中至少会有一次回流，无法避免的）
* 添加删除dom
* 元素尺寸或位置变化
* 元素内容变化（文字数量、图片大小）
* 元素字体大小
* 浏览器窗口大小更改，触发resize


## 重绘（Repaint）
render tree中的节点的一些几何属性或样式的修改只是影响到外观，不影响布局，会发生重绘。在构造渲染书和回流阶段之后，浏览器知道了哪些节点是可见的，以及节点的大小位置和样式等信息，将节点转换为屏幕上的元素的过程即为重绘。
比如修改`visibility`、`color`、`background-color`

## 浏览器优化
因为回流重绘过程耗性能，现代浏览器对此做了相应的优化。大部分浏览器会维护一个队列，将一系列重绘回流操作放入更新队列。当任务数量达到一定数量或时间间隔达到阈值，会将队列清空，进行批处理更新。但当我们访问以下属性或方法时，浏览器会立刻清空队列。  

一些触发回流的属性和方法：
* `offsetWidth`、`offsetHeight`、`offsetTop`、`offsetLeft`
* `scrollWidth`、`scrollHeight`、`scrollTop`、`scrollLeft`
* `clientWidth`、`clientHeight`、`clientTop`、`clientLeft`
* `getcomputedstyle()`
* `getboundingclientrect()`

所以尽量少使用这些属性或方法。

## 减少回流重绘

### 样式
比如
``` javascript
const el = document.getElementById('app');
el.style.margin = '10px';
el.style.borderTop = '2px';
el.style.borderLeft = '1px';
```
当然之前说了现代浏览器对这种情况会维护一个更新队列，但旧的浏览器就会触发三次回流。所以对上面的写法可以改为一次性添加或维护一个class。

避免使用css表达式，比如`calc`。
在dom树末端更改class，避免设置多层内联样式。

### dom
需要尽可能的减少dom的修改。对于dom的操作，可以通过以下步骤减少重绘回流次数：
1. 脱离文档流
2. 修改dom
3. 将修改后的dom重新加回文档流

脱离文档流的方式有：
1. 隐藏元素
``` javascript
function appendDataToElement(appendToElement, data) {
    let li;
    for (let i = 0; i < data.length; i++) {
        li = document.createElement('li');
        li.textContent = 'text';
        appendToElement.appendChild(li);
    }
}
const ul = document.getElementById('list');
ul.style.display = 'none';
appendDataToElement(ul, data);
ul.style.display = 'block';
```

2. 使用文档片段Document Fragment
``` javascript
const ul = document.getElementById('list');
const fragment = document.createDocumentFragment();
append(fragment, data);
ul.appendChild(fragment);
```
3. 将元素拷贝到一个脱离文档的节点中
``` javascript
const ul = document.getElementById('list');
const clone = ul.cloneNode(true);
appendDataToElement(clone, data);
ul.parentNode.replaceChild(clone, ul);
```
以上例子参考[这篇文章](https://juejin.im/post/5c3c023b518825251077c679#heading-9)

应避免使用table元素，这玩意渲染有点不一样。

### GPU加速
GPU硬件加速，可以让transform\opacity、filters这些动画不会引起重绘回流。但需要合理开启，避免引起内存占用过高的性能问题。