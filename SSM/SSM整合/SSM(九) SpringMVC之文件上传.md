# SpringMVC的文件上传的两个解析类

SpringMVC中，文件的上传，是通过 MultipartResolver 实现的。 所以如果要实现文件的上传，要在 spring-mvc.xml 中注册相应的 MultipartResolver。

Spring提供了两个MultipartResolver的实现类用于处理multipart请求

> CommonsMultipartResolver</br>
> StandardServletMultipartResolver

二者区别：

- CommonsMultipartResolver需要使用commons Fileupload来处理multipart请求，所以在使用时，必须要引入相应的jar包：commons-fileupload和commons-io

- StandardServletMultipartResolver是基于Servlet3.0来处理multipart请求的，所以并不需要引用其他jar包，但是必须要要使用支持Servlet3.0的容器才可以，
并且Servlet也必须是3.0


## CommonsMultipartResolver的使用

springmvc配置

## StandardServletMultipartResolver的使用
