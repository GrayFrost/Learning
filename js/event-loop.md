# 事件循环
执行栈中的任务 -> 微任务 -> 宏任务

## 宏任务
Macro

settimeout
setinterval
setimmediate

## 微任务
Micro

promise.then
mutationobserver
process.nexttick

## Node中的事件循环