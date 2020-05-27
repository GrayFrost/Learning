# React Fiber

使用fiber后的一些新api

requestIdleCallback


60fps
一帧16ms，在一帧里浏览器需要完成：
执行js
计算style
构建布局模型
绘制图层样式
渲染

如果整个流程超过16ms，出现卡顿

react旧版更新方案，同步更新。如果更新一个组件1ms，1000个组件1s

时间切片 耗时长的任务切片，非阻塞渲染

reconciliation
render commit