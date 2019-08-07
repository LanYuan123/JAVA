当浏览器发送请求到服务器时，服务器会自动创建HttpServletRequest对象并且传入Servlet，该对象储存了浏览器发送到服务器端的所有信息，我们可以通过该对象获取
我们想要获得的信息

### :pear:request对象的使用:
   1. #### 获取请求行的数据
      - 获取协议：req.getScheme();
      - 获取URL：req.getRequsetURL();
      - 获取URI：req.getRequestURI();
      - 获取请求方式：req.getMethod();
   2. #### 获取请求头数据
      - 获取值：req.getHeader(String key)；
      - 获取所有的键：req.getHeaderName();  返回枚举类型
   3. #### 获取用户数据
      - 获取键的值：req.getParameter(String key);
      - 获取所有键名：req.getParameterNames();   返回枚举类型
      - 获取一个键对应多个值的键值：req.getParameterValues();    返回一个String数组
      
 ### :pear:request对象的作用域
 
  - 基于**请求转发**，**一次请求中**的**所有Servlet**之间的数据流转与共享</br>
  - 除了request对象自带的浏览器传来的信心，我们也可以自己像request对象中添加信息，一样是按照键值对的形式添加。</br>
  - 添加信息：req.setAttribute(String key,String value);</br>
  - 获取新信息：req.getAttribute(String key);
    

