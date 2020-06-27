# 性能优化

## performance

### performance.memory
Chrome中的一个非标准属性，获取内存的基本使用情况
``` javascript
performance.memory = {
  jsHeapSizeLimit, // 内存大小限制
  totalJSHeapSize, // 可使用内存大小
  usedJSHeapSize // js对象占用的内存大小
}
```

### performance.navigation
提供页面操作信息
``` javascript
performance.navigation = {
  redirectCount, // 达到该页面之前经过多少次重定向
  type// 有四个值：0 1 2 225
}
```

### performance.timing
检测性能相关

performance.now()
performance.mark()

## 方式
code spliting
异步加载

代码压缩
tree shaking
gzip

缓存

内容方面
cdn

网络方面
缓存
文件压缩
按需加载
减少cookie大小
文件合并，雪碧图
文件预加载，图片懒加载

渲染方面
js放底部，css放顶部
减少重绘回流
减少dom节点

代码方面
减少dom节点操作
css选择器优化
事件委托