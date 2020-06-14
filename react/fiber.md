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

stack reconciler   基于树的深度遍历
fiber reconciler 单链表
可拆分，可中断任务
重用各阶段任务，可以设置优先级
父子组件任务间前进后退切换任务  ？
render可以返回多元素（即可以返回一个数组）
支持异常边界处理


fiber把递归diff进行拆分

fiber tree workInProgress

## 总结
react组件渲染分为两个阶段：reconciler、render
reconciler阶段是对virtual dom的操作，根据对应的调度算法对virtual dom进行更新。原来使用stack reconciler，现在为fiber reconciler
render阶段是渲染阶段，拿到更新后的结果，根据所处应用环境的不同，使用不同的渲染方式进行渲染。环境有web、native、服务端