# Session

Cookie 以明文的方式存储了用户信息，造成了非常大的安全隐患，而 Session 的出现解决这个问题。用户信息可以以 Session 的形式存储在后端。这样当用户请求到来时，请求可以和 Session 对应起来，当后端处理请求时，可以从 Session 中获取用户信息。那么 Session 是怎么和请求对应起来的？答案是通过 Cookie，在 Cookie 中填充一个类似 SessionID 之类的字段用来标识请求。这样用户的信息存在后端，相对安全，也不需要在 Cookie 中存储大量信息浪费流量。但前端想要获取用户信息，例如昵称，头像等信息，依然需要请求后端接口去获取这些信息。

通过Cookie技术和Session的结合使用，使得服务端在HTTP协议即使是无状态的情况下，也可以标识到不同的用户，并且在请求之间建立了联系，从而提供一些个性化服务。

**特点：**

- 存储在服务器端
- 服务器进行创建
- 依赖Cookie技术
- 声明周期为一次会话
- 默认存储时间是30分钟
    
## 使用：

用户第一次访问服务器，服务器会创建一个session对象给此用户，并将该session对象的JESSIONID使用Cookie技术存储到浏览器中，保证用户的其他请求能够获取到同一个session对象，也保证了不同请求能够获取到共享的数据

## 常用API

- **创建session对象/获取session对象**
  
  ```
  HttpSession session = req.getSession()
  ```
  
  这一条语句既可以创建Session对象，也可以获取Session对象
  
  - 创建Session对象：
    - 如果请求中拥有JSESSIONID，则返回其对应的session对象
    - 如果请求中没有JSESSIONID，则创建新的session对象，并将JSESSIONID作为Cookie信息创建，并且响应
    - 如果session对象是失效了，也会创建一个session对象，并将其JSESSIONID存储在浏览器内存之中
    - 创建好Session对象之后，该语句也自动将JSESSIONID添加到Request的Cookie之中
  - 获取session对象
    - 如果Request对象中的cookie含有JSESSIONID，则获取该session对象
    - 如果没有则创建新的session对象

- **设置session存储时间**

  默认存储时间是30分钟,如果在指定的时间内session对象没有被使用则被销毁，如果使用了则重新计时
  
  ```
  session.setMaxInactiveInterval(int seconds);
  ```

- **设置session强制失效**
  
  ```
  hs.invalidate();
  ```
  
