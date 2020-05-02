# 重绘与回流

## 浏览器渲染机制
html解析成DOM，css解析成CSSOM，将DOM和CSSOM结合，得到渲染书（render tree），根据render tree，计算所有节点的大小和位置，最后渲染到页面上。

回流一定会引起重绘，重绘不一定会引起回流。

## 回流（Reflow）
render tree 的部分或全部元素发生尺寸、布局的改变时，会发生回流。回流就是在渲染树计算节点在视窗的大小和位置的过程。

会导致回流的一些操作：
页面初始化（所以项目中至少会有一次回流，无法避免的）
添加删除dom
元素尺寸或位置变化
元素内容变化（文字数量、图片大小）
元素字体大小
浏览器窗口大小，resize


## 重绘（Repaint）
render tree中的节点的一些几何属性或样式的修改只是影响到外观，不影响布局，会发生重绘。在构造渲染书和回流阶段之后，浏览器知道了哪些节点是可见的，以及节点的大小位置和样式等信息，将节点转换为屏幕上的元素的过程即为重绘。
比如修改visibility、color、background-color

## 浏览器优化
因为回流重绘过程耗性能，现代浏览器对此做了相应的优化。大部分浏览器会维护一个队列，将一系列重绘回流操作放入更新队列。当任务数量达到一定数量或时间间隔达到阈值，会将队列清空，进行批处理更新。但当我们访问以下属性或方法时，浏览器会立刻清空队列。

一些触发回流的属性和方法：
offsetWidth、offsetHeight、offsetTop、offsetLeft
scrollWidth、scrollHeight、scrollTop、scrollLeft
clientWidth、clientHeight、clientTop、clientLeft
getcomputedstyle
getboundingclientrect

所以尽量少使用这些属性或方法。

## 减少回流重绘
一次性写style
documentfragment
display:none
避免使用table
避免使用css 表达式 calc
dom树末端更改class，避免设置多层内联样式