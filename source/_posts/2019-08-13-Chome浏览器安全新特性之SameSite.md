---
title: Chome浏览器安全新特性之SameSite
date: 2019-08-13 08:56:48
tags: 
- cookie
- CSRF
- SameSite
categories: 其他前端
---

chrome浏览器研发团队宣布将会对用户隐私和安全的处理机制作出重大改变

<!-- more -->

## Chrome新特性

chrome浏览器将会在今年晚些的版本中，提供一些新特性：

1. 在**默认**情况下阻止网站的**cookies跨域传输**。开发者必须通过**手动设置**cookie的`SameSite`属性，决定是否允许此cookie跨域传输。

2. 提供**哪些站点正在设置cookie的明确信息**，以便用户可以对其数据的使用方式做出明智的选择。

3. **跨站点cookie传输只能作用在HTTPS域名。**

4. 限制网站获取**除cookie信息之外**的信息。这些信息是由`UA`，`referer`请求头等信息组成的，包含了大量的用户独特信息。

## 安全因素

`cookies`使得基于**无状态的HTTP协议**记录稳定的状态信息成为了可能。但它也是不安全的起因，有以下几点：

1. cookie包含了用户在某站点的身份证明。浏览器中的document对象中，储存了Cookie的信息，如果利用js可以把这里面的cookie给取出来（`document.cookie`），只要得到此cookie拥有者的的身份。

2. cookie可以**跨站点和域名传输**。这是整个业界进行**商业化广告精准投放**的技术基础。这也是为什么你在淘宝搜索某类商品之后，第三方网页广告能否给你推荐你搜索过的这类商品的原因。

## 网络攻击方式

最常见的二大类攻击方式，均涉及到了对用户cookies的盗取和伪造：

1. XSS(Cross SiteScript) 跨站脚本攻击
2. CSRF (Cross Site Request Forgery) 跨域请求伪造

cookie 最初被设计成了允许在**第三方网站发起的请求中携带**，`CSRF`攻击就是利用了cookie的这一“弱点”，举个栗子：

当某用户通过登录正常合法进入了`a.com`之后，就获得了`a.com`的登录凭证（Cookie）。这些由`a.com`内页面发起的请求的 URL **不一定也是指向`a.com` 上**，可能有指向 b.com 的，也可能有指向 c.com 的。我们把发送给 `a.com` 上的请求叫做**第一方请求**（first-party request），发送给 b.com 和 c.com 等的请求叫做**第三方请求**（third-party request）， 第三方请求和第一方请求一样，都会带上各自域名下的 cookie，所以就有了第一方 cookie（first-party cookie）和第三方 cookie（third-party cookie）的区别。上面提到的 CSRF 攻击，就是利用了第三方 cookie 。

### CSRF攻击流程

1. 受害者登录a.com，并保留了登录凭证（Cookie）。

2. 攻击者引诱受害者访问了b.com。

3. b.com 向 a.com 发送了一个请求：a.com/act=xx。**浏览器会默认携带a.com的Cookie。**

4. a.com接收到请求后，对请求进行验证，并确认是受害者的凭证，**误以为是受害者自己发送的请求。**

5. a.com以受害者的名义执行了act=xx。

攻击完成，攻击者在受害者不知情的情况下，冒充受害者，让a.com执行了自己定义的操作。

### CSRF防御策略

目前在业界防御 CSRF 攻击主要有三种策略：

1. 验证 HTTP **Referer** 字段，必须同源请求才认为是安全的。
2. 在请求地址中添加 **token** 并验证。
3. 在 HTTP 头中**自定义属性并验证**。

在2016年5月，Google起草了一份[草案](https://tools.ietf.org/html/draft-west-first-party-cookies-07)来改进HTTP协议。在2016年的chrome 51版本中为Set-Cookie响应头新增[SameSite](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie)属性，它用来标明这个 Cookie是个“同站 Cookie”，来控制原本“第三方cookie”带来的安全风险。

## `SameSite`属性

可以参考阮老师的[这篇文章](http://www.ruanyifeng.com/blog/2019/09/cookie-samesite.html)。

使用SameSite这个属性时：需要在Set-Cookie中加入SameSite属性，并可以对其赋予几种值，例如：

```
Set - Cookie : key = value ; HttpOnly ; SameSite = Strict
```

### 严格模式

```
SameSite=Strict
```

含义为：此cookie为禁止发送所有第三方链接的cookie。
当我们通过其他网站来访问一个有 SameSite-Cookies 机制的网站时，例如从 a.com 点击链接进入 b.com，如果 b.com 设置了 Set-Cookie:foo=bar;SameSite=Strict，那么，foo=bar 这一 cookie 是不会随着 request 发送的，这就防御了CSRF。

### 宽松模式

```
SameSite=LAX
```

![宽松模式](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2019-9-10-17-9-4.png)

宽松模式下也不是允许所有类型的跨域请求都可以携带cookie，限制有以下几类：

1. 允许发送安全 HTTP 请求类型（ GET , HEAD , OPTIONS , TRACE ）第三方链接的 cookies。

2. 必须是 TOP-LEVEL 即可引起地址栏变化的跳转方式，例如`<a> , <link rel = "prerender" > ，以及GET 方式的 form 表单 ，此外， XMLHttpRequest , 图标标签的地址`等方式进行 GET 方式的访问将不会发送 cookies。

3. 发送不安全 HTTP 方法（ POST , PUT , DELETE ）类型的http 请求被视为不安全的的请求，禁止携带第三方链接的cookies。

此外，值得说明的是：如果我们给cookie添加了SameSite关键字，但是没有指定 value（Lax or Stict），例如
`Set - Cookie : key = value ; HttpOnly ; SameSite`
此种情况下，chrome浏览器会认为等同于
`Set - Cookie : key = value ; HttpOnly ; SameSite = Strict`
以上即是截止到目前对于cookie的处理方式。
