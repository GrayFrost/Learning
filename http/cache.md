# http缓存

## 存储位置
service worker
memory cache
disk cache
push cache

## 强缓存
与强缓存相关的header有：expires、Cache-Control。
expires 返回GMT时间。存在服务器时间与客户端时间不统一的问题。

cache-control
max-age
public
private
no-cache
no-store

## 协商缓存
Last-Modified/If-Modified-Since
因为last-modified返回的时间是秒级的，如果一个文件在一秒之内改动了多次

Etag/If-None-Match
etag的生成

## 流程
缓存的流程
