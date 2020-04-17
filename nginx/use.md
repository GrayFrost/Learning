# Nginx

## 安装
我们直接在Mac安装。
首先确保已安装Homebrew。

``` shell
brew install nginx
```
查看是否安装成功
``` shell
nginx -v
```
在我的终端里出现了`nginx version: nginx/1.17.9`，已安装成功。

## 使用

启动Nginx
``` shell
brew services start nginx
```
或直接
```
nginx
```
打开localhost:8080，可以正常访问。

关闭nginx
快速停止nginx
``` shell
nginx -s stop
```

完整有序的停止nginx
``` shell
nginx -s quit
```

其他停止方式
``` shell
ps -ef | grep nginx
```
在列出来的信息中查找nginx:master对应的主进程号，然后

从容停止Nginx
``` shell
kill -QUIT 主进程号
```

或快速停止Nginx
``` shell
kill -TERM 主进程号
```