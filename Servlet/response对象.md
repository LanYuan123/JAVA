服务器对浏览器的请求做完处理之后，将会对浏览器做出响应，响应的内容都储存在HttpServletResponse对象中

### :pear:response对象的使用
   1. #### 设置响应头
      - resp.setHeader(Strring name,String value);   该方法如果对同一个键设置两个值后面赋的值将会覆盖前面的值
      - resp.addHeader(String name,String value);   该方法如果对同一个键设置两个值，那么该键会有两个值，通常用于多选框
   2. #### 设置编码格式
      - resp.setContentType(String s)，也可以使用resp.setHeader(String key,String value);
   3. #### 设置响应状态码
      - resp.sendError(int i ,String s);
   4. #### 设置响应数据
      - resp.getWriter().write();   相当于在网页上打印
