# 客户端识别与 cookie 机制

**先来看看没用 cookie ，早期的 web 识别客户端的方法**

* 用户信息首部

![Image text](/images/1648124119(1).png)

* 客户端 IP 地址

早期的 web 先锋曾尝试着将客户端 IP 地址作为一种标识形式使用。如果每个用户 IP 地址都不同也不经常发生变化，这个方案是可行的。通过 IP 地址识别用户的缺点也很明显的。

* 用户登录

通过用户登录用户名和密码来进行认证显式的询问用户是谁。

为了使用 Web 站点的登录更加简便，HTTP 中包含了一种内建机制，可以用 WWW-Authenticate 首部和 Authorization 首部向 Web 站点传送用户的相关信息。

![Image text](/images/1648124709(1).png)

* 胖 URL

有些网站会为用户生成特定的 URL 来追踪用户身份，例如在 URL 后面加一个用户 ID。改动后包含了用户状态信息的 URL 称为胖 URL。

## cookie

cookie 是当前识别用户，实现持久会话的最好方式。前面的提到各种技术存在很多问题。

### cookie 的类型

可以笼统的将 cookie 分为两类：

1. 会话 cookie, 一种临时 cookie 记录用户访问站点时的设置和偏好。用户关闭浏览器，会话 cookie 就被删除了。
2. 持久 cookie 的生存时间更长些，他们储存在磁盘上，浏览器关闭，计算机重启依然会存在，持久 cookie 通常是维护用户周期性的访问站点。

### cookie 是如何工作的

客户端在用户登录请求后，服务器会返回一个响应，并在首部添加一个 Set-Cookie 的首部，cookie 都是键值对的形式。在接下来的请求中浏览器都会自动带上一个 Cookie 首部，值是 Set-Cookie 返回的值，服务器会验证  Cookie 值来确认当前用户。

![Image text](/images/1648125866(1).png)

### cookie 罐：客户端的状态

cookie 的基本思想就是让浏览器存储一些服务器特有的信息，每次访问服务器都把这些信息提供给它。cookie 规范的正式名称为 HTTP 状态管理机制。 不同的浏览器有可能会将 cookie 存储在不同的文件夹。

### 不同站点使用不同的 cookie

浏览器只向服务器发送服务器产生的那些 cookie，也就是说 A 网站的服务器只会获得 A 网站的 cookie，而不会是别的网站。

#### cookie 的域属性

产生 cookie 的服务器可以向 Set-cookie 响应首部添加一个 Domain 属性来控制哪些网站可以查看 cookie。

#### cookie 的路径属性

cookie 规范允许用户将 cookie 与部分网站关联起来。可以通过 Path 属性来实现这一功能，在这个属性列出的 URL 路径前缀下所有 cookie 都是有效的。

### cookie 成分

cookie 规范有两个不同的版本：版本 0 和 版本 1, 版本 1 是对版本 0 的扩展。

#### 版本 0

![Image text](/images/1648126670(1).png)

![Image text](/images/1648126717.png)

#### 版本 1

![Image text](/images/1648126884(1).png)

![Image text](/images/1648126915(1).png)

## cookie 与会话跟踪

可以用 cookie 在用户与某个 Web 站点进行多项事务处理时对用户进行跟踪。电子商务 Web 站点用会话 cookie 在用户浏览时记录下用户的购物车的信息。通过临时 cookie 来跟踪用户会话，最后拿到用户购物车信息给下一步操使用。