# 事件循环
js单线程，在处理各种行为时，为了不阻塞主线程，使用事件循环实现异步。

执行栈中的任务 -> 微任务 -> 宏任务

## 宏任务
MacroTask

settimeout
setinterval
requestAnimationFrame
setimmediate
I/O

## 微任务
MicroTask

Promise.then catch finally
Object.observe（已废弃）
MutationObserver
process.nextTick

## 事件循环
js 主线程 同步任务    任务队列 异步任务
同步任务在主线程上执行，形成执行栈
主线程外，任务队列
执行栈中的任务执行完毕，读取任务队列
js单线程 主线程 执行栈

执行一个宏任务，遇到微任务，加入微任务队列，然后执行完微任务队列里的所有任务，再开启下一个循环。
先执行宏任务，然后执行该宏任务产生的微任务，若微任务在执行过程中产生了新的微任务，继续执行微任务，微任务执行完后，进入一下轮循环

script会看成是一个macrotask，事件循环先从macrotask开始，然后运行微任务队列中的所有任务，然后再开启下一个循环。

## Node中的事件循环
输入数据 轮询 检查 关闭  定时器 io 闲置

timers：执行setTimeout和setInterval设置的回调
pending callbacks
idle, prepare
poll 
check: 执行setImmediate回调
close callbacks

process.nextTick优于其他microtask执行