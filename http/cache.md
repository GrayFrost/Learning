# http缓存

## 存储位置
service worker
memory cache
disk cache
push cache

## 强缓存
使用强缓存，不会发送请求。请求得到的响应状态码是200，数据从缓存中拿，数据大小中显示`from cache`。

### Expires
与强缓存相关的header有：`Expires`、`Cache-Control`。
返回的是GMT时间。存在服务器时间与客户端时间不统一的问题。

### Cache-Control
Cache-Control常使用的值：
* max-age 缓存的存货时间，单位秒。
* no-cache 可以缓存，缓存立即失效，需要使用协商缓存进行验证
* no-store 禁用缓存
* public 客户端和代理服务器都能缓存
* private 只能在客户端中缓存。

曾做过测试，当设置了`Cache-Control`的值为`no-cache`，而`Expires`设置了一个过期时间时，仍然可以进行缓存。所以应该说是当`Cache-Control`设置了`max-age`值时，才覆盖`Expires`的值。

## 协商缓存
使用协商缓存，会发送请求，请求得到的响应状态码是304，服务器不会发送数据给客户端，只是返回一个304状态码，让客户端直接从缓存中拿数据。

### Last-Modified
相关字段：
* Last-Modified
* If-Modified-Since

服务器返回Last-Modified响应头，表示一个文件的更改时间。比如说在node中，可以返回`mtime`。
在下一次请求时，请求的请求头中会在If-Modified-Since的头中携带上从Last-Modified中拿到的值，再去与服务器中的资源的文件修改时间进行比较。  
因为last-modified返回的时间是秒级的，如果一个文件在一秒之内改动了多次，那用If-Modified-Since的值和Last-Modified的值相同，导致没有及时拿到最新更改的数据，仍旧使用了缓存。针对这个问题，使用Etag。

### Etag
相关字段：
* ETag
* If-None-Match

服务器返回ETag，这是一个文件的唯一标识。在下一次请求时，请求的请求头中会在If-None-Match中携带上从ETag中拿到的值，再去与服务器中的资源的文件标识进行比较。ETag是由服务器计算出来的，如果是根据文件内容来算的，当涉及到大文件时，需要谨慎使用。    
当响应头中同时存在ETag和Last-Modified时，ETag的优先级更高。


## 流程
首先检查发送请求的请求头中是否携带了跟强缓存相关的请求头，有则判断是否命中缓存，命中则使用缓存。没有命中的话，检查请求头中是否携带了跟协商缓存相关的请求头，携带了则去服务器中与相应的字段做比较，判断文件是否更改。如果服务器的文件更新过了，返回新的资源，同时在相应头中携带新的资源的更新信息，方便下一次请求时携带。没有更新则返回304状态码。
