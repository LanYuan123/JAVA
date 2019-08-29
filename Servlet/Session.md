# Session
    解决了一个用户不同请求的数据共享问题
    
### 原理：
&nbsp;&nbsp;&nbsp;&nbsp;用户第一次访问服务器，服务器会创建一个session对象给此用户，并将该session对象的JESSIONID
使用Cookie技术存储到浏览器中，保证用户的其他请求能够获取到同一个session对象，也保证了不同请求能够获取到共享的数据

### 特点：
&nbsp;&nbsp;&nbsp;&nbsp;存储在服务器端

&nbsp;&nbsp;&nbsp;&nbsp;服务器进行创建

&nbsp;&nbsp;&nbsp;&nbsp;依赖Cookie技术

&nbsp;&nbsp;&nbsp;&nbsp;一次会话

&nbsp;&nbsp;&nbsp;&nbsp;默认存储时间是30分钟

### 使用：

- **创建session对象/获取session对象**

HttpSession hs = req.getSession();</br>

如果请求中**拥有session的标识符**也就是JSESSIONID，则**返回其对应的session对象**</br>

如果请求中**没有session的标识符**也就是JSESSIONID，则**创建**新的session对象，并将JSESSIONID作为Cookie信息创建，并且响应

如果**session对象是失效**了，也会**创建一个session对象**，并将其JSESSIONID存储在浏览器内存之中

- **设置session存储时间**

默认存储时间是30分钟

hs.setMaxInactiveInterval(int seconds);

如果在指定的时间内session对象没有被使用则被销毁，如果使用了则重新计时

- **设置session强制失效

hs.invalidate();

