# Lazy Man
Lazy Man题目如下：
```
实现一个LazyMan，可以按照以下方式调用:
LazyMan(“Hank”)输出:
Hi! This is Hank!

LazyMan(“Hank”).sleep(10).eat(“dinner”)输出
Hi! This is Hank!
//等待10秒..
Wake up after 10
Eat dinner~

LazyMan(“Hank”).eat(“dinner”).eat(“supper”)输出
Hi This is Hank!
Eat dinner~
Eat supper~

LazyMan(“Hank”).sleepFirst(5).eat(“supper”)输出
//等待5秒
Wake up after 5
Hi This is Hank!
Eat supper
以此类推。
```
本题可以涉及到的知识点有：队列、链式调用、事件循环

``` javascript
class _LazyMan {
    constructor(name) {
        this.name = name;
        this.tasks = [];
        const task = () => {
            console.log(`Hello ${this.name}`);
            this.next();
        };
        this.tasks.push(task);
        setTimeout(() => {
            this.next();
        }, 0); // 等待添加完事件
    }
    next() {
        const task = this.tasks.shift();
        task && task();
    }
    eat(food) {
        const task = () => {
            console.log(`eat ${food}`);
            this.next();
        };
        this.tasks.push(task);
        return this; // 返回this，链式调用
    }
    sleep(second) {
        const task = () => {
            setTimeout(() => {
                console.log(`sleep ${second}s`);
                this.next();
            }, second * 1000);
        };
        this.tasks.push(task);
        return this;
    }
    sleepFirst(second) {
        const task = () => {
            setTimeout(() => {
                console.log(`sleep ${second}s first`);
                this.next();
            }, second * 1000);
        };
        this.tasks.unshift(task);
        return this;
    }
}

function LazyMan(name) {
    return new _LazyMan(name); // 工厂模式，内部封装实例化
}
```