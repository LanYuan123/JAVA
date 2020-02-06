# URL路径匹配

## /

**会匹配到 /springmvc 这样的路径型url，不会匹配到模式为\*.jsp这样的后缀型url**

## /*

**会匹配所有的url,路径型的和后缀型的url(包括/springmvc，.jsp，.js和\*.html等)的URL**

## /*.action

**会匹配相应后缀（例如.action）的URL**

</br>
</br>

> 关于这个问题在StackOverflow上有更详细的解释：[https://stackoverflow.com/questions/4140448/difference-between-and-in-servlet-mapping-url-pattern](https://stackoverflow.com/questions/4140448/difference-between-and-in-servlet-mapping-url-pattern)
