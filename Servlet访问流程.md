# Servlet的访问流程
 1. 浏览器发起请求到服务器<br/>
 2. 服务器接收到浏览器的请求进行解析，创建HttpServletRequest对象存储数据<br/>
 3. 服务器调用对应的Servlet进行处理，并将HttpServletRequest的实例对象作为参数传递给Servlet中的方法进行处理<br/>
 4. Servlet中的方法进行请求处理<br/>
