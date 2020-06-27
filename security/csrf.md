# CSRF

`CSRF`(Cross-Site-Request-Forgery) 跨站请求伪造

以受害者的身份伪造请求发送给服务器
使用用户的登录态发起恶意请求

## 过程
登录受信任网站A，并在本地生成cookie。用户在不登出网站A的情况下，访问危险网站B。

两个特点：
* CSRF通常发生在第三方域名
* CSRF攻击者不能获取Cookie信息，只是使用

## 防范
* 验证码

  `CSRF`往往在用户不知情的情况下发生，使用验证码，强制用户与网站进行交互。

* SameSite

  cookie设置`SameSite`属性

* Referer

  标明请求来源

* Token