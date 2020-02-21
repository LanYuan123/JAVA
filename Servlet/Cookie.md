# Cookie技术

要了解Cookie技术我们先要从HTTP协议说起：

## HTTP协议

我们先从 HTTP 协议说起。HTTP 协议有个特点，是无状态的，意味着请求与请求是没有关系的。早期的 HTTP 协议只是用来简单地浏览网页，没有其他需求，因此请求与请求之间不需要关联。但现代的 Web 应用功能非常丰富，可以网上购物、支付、游戏、听音乐等等。如果请求与请求之间没有关联，就会出现一个很尴尬的问题：Web 应用不知道您是谁。例如，用户登录之后在购物车中添加了三件商品到购物车，刷新一下网页，用户仍然处于未登录的状态，购物车里空空如也。很显然这种情况是不可接受的。为此 HTTP 协议急需一种技术让请求与请求之间建立起联系来标识用户。于是出现了 Cookie 技术。

## Cookie技术

Cookie 是 HTTP 报文的一个请求头，Web 应用可以将用户的标识信息或者其他一些信息（用户名等等）存储在 Cookie 中。用户经过验证之后，每次 HTTP 请求报文中都包含 Cookie；当然服务端为了标识用户，即使不经过登录验证，也可以存放一个唯一的字符串用来标识用户。采用 Cookie 就解决了用户标识的问题，同时 Cookie 中包含有用户的其他信息。Cookie 本质上就是一份存储在用户本地的文件，里面包含了需要在每次请求中传递的信息。

**Cookie有以下特点**：
    
    - Cookie是浏览器端的数据储存技术
    - 存储的数据在服务器端声明，但是是存储在浏览器端的
    - 可以临时存储，也可以定时存储
    - 临时存储：存储在浏览器的运行内存中，浏览器关闭时失效
    - 定时存储：设置了Cookie的有效期，存储在客户端的硬盘中，在有效期内符合路径要求的请求都会附带该信息
    - 当没有设置时，默认当Cookie储存好之后，项目的每次请求都会附带该信息，除非设置有效路径

**Cookie存在以下缺点**:
    
    - Cookie 具有时效性，超过过期时间就会失效。
    - 服务提供商利用 cookie 恶意搜集用户信息，例如用户在未登录的情况下去商城浏览了商品，商城就会把广告公司的 Cookie 加入到用户的浏览器中，每当用户浏览和广告公司合作的网站时，都会看到之前在商城浏览过的类似商品。
    - 每次 Cookie 都会把除用户标识之外的其他用户信息也在 Cookie 中传递，增加了请求的流量开销。
    
通过Cookie技术和Session的结合使用，使得服务端在HTTP协议即使是无状态的情况下，也可以标识到不同的用户，并且在请求之间建立了联系，从而提供一些个性化服务。


## 常用API：

**创建Cookie**，一条cookie储存一条数据，多条数据需要多创建几个Cookie对象进行储存

作用 | 方法
---|:--:
创建Cookie | Cookie cookie = new Cookie(String key,String value);

**设置Cookie的时间与路径信息**，Cookie的定时储存是针对该文件夹下所有的路径，如果希望只对某一路径，则需设置有效路径

作用 | 方法
---|:--:
设置有效期 | cookie.setMaxAge(int seconds);
设置有效路径 | cookie.setPath(String path);

**获取Cookie**

作用 | 方法
---|:--:
获取Cookie信息数组 | Cookie cookie[] = req.getCookies(); 

**响应Cookie给客户端**

作用 | 方法
---|:--:
响应Cookie信息给客户端|resp.addCookie(Cookie cookie);

> 参考文章：[详解 Spring Session 架构与设计](https://www.ibm.com/developerworks/cn/web/wa-spring-session-architecture-and-design/index.html)
