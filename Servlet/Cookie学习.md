# Cookie
    解决了发送的不同请求的数据共享问题
    
### 使用：
&nbsp;&nbsp;&nbsp;&nbsp;//创建Cookie对象</br>
&nbsp;&nbsp;&nbsp;&nbsp;Cookie cookie = new Cookie（key,value）;</br>
&nbsp;&nbsp;&nbsp;&nbsp;//响应Cookie信息给客户端</br>
&nbsp;&nbsp;&nbsp;&nbsp;resp.addCookie(Cookie cookie); </br>

### 注意：
&nbsp;&nbsp;&nbsp;&nbsp;一条cookie储存一条数据，多条数据需要多创建几个Cookie对象进行储存

### 特点：
&nbsp;&nbsp;&nbsp;&nbsp;Cookie是浏览器端的数据储存技术</br>

&nbsp;&nbsp;&nbsp;&nbsp;存储的数据在服务器端声明，但是是存储在浏览器端的</br>

&nbsp;&nbsp;&nbsp;&nbsp;**临时存储**：存储在浏览器的运行内存中，浏览器关闭时失效</br>

&nbsp;&nbsp;&nbsp;&nbsp;**定时存储**：设置了Cookie的有效期，存储在客户端的硬盘中，在有效期内符合路径要求的请求
都会附带该信息</br>

&nbsp;&nbsp;&nbsp;&nbsp;当没有设置时，默认当Cookie储存好之后，项目的每次请求都会附带该信息，除非设置有效路径</br>

# Cookie的定时存储

### 注意：
&nbsp;&nbsp;&nbsp;&nbsp;在使用setMaxAge（int seconds）对cookie内容进行定时储存的时候，
setMaxAge（）要使用在addCookie（）之前

&nbsp;&nbsp;&nbsp;&nbsp;Cookie的定时储存是针对该文件夹下所有的路径，如果希望只对某一路径，则需设置有效路径

&nbsp;&nbsp;&nbsp;&nbsp;使用setPath(String path)来设置有效路径</br>
&nbsp;&nbsp;&nbsp;&nbsp;Path是绝对路径

### 使用：

创建Cookie

作用 | 方法
---|:--:
创建Cookie | Cookie cookie = new Cookie(String key,String value);

设置Cookie

作用 | 方法
---|:--:
设置有效期 | cookie.setMaxAge(int seconds);
设置有效路径 | cookie.setPath(String path);

获取Cookie

作用 | 方法
---|:--:
获取Cookie信息数组 | Cookie cookie[] = req.getCookies(); 
