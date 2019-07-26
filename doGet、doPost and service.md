1. ### doGet，doPost and Service方法的作用

   :pushpin: **doGet方法**：当浏览器传来的请求为get请求时，服务器会调用Servlet中的daGet方法进行处理
    
   :pushpin: **doPost方法**：当浏览器传来的请求为post请求时，服务器会调用Servlet中的daPost方法进行处理
    
   :pushpin: **Service方法**：当浏览器传来请求时，如果Servlet中存在Service方法
   
2. ### doGet，doPost and Service三者的区别

   - #### Service方法的优先级是最高的，当Service方法存在于Servlet中的时候，不管是Get请求还是Post请求，都会优先进入Service方法进行处理
   - #### 如果覆写的Service方法中调用了父类的Service方法（super.service(req,resp)）,那么在执行到service的父类service方法的时候，会按照请求的方式（get或者post）来调用doGet或者doPost
   - #### 注意：如果在复写的service方法中调用了父类的service方法（super.service(req,resp)），但是在该servlet类中没有doGet和doPost方法，会报405的错
   - #### 所以只要Service存在的时候，servlet一定是先执行Service方法的，当Service方法不存在时，Servlet会根据请求方式的不同来调用相应的doGet方法和doPost方法
   
