# SpringMVC的文件上传的两个解析类

SpringMVC中，文件的上传，是通过 MultipartResolver 实现的。 所以如果要实现文件的上传，要在 spring-mvc.xml 中注册相应的 MultipartResolver。

Spring提供了两个MultipartResolver的实现类用于处理multipart请求

> CommonsMultipartResolver</br>
> StandardServletMultipartResolver

二者区别：

- CommonsMultipartResolver需要使用commons Fileupload来处理multipart请求，所以在使用时，必须要引入相应的jar包：commons-fileupload和commons-io
- StandardServletMultipartResolver是基于Servlet3.0来处理multipart请求的，所以并不需要引用其他jar包，但是必须要要使用支持Servlet3.0的容器才可以，并且Servlet也必须是3.0
- 现在推荐使用tandardServletMultipartResolver，在低版本下可以使用CommonsMultipartResolver


## CommonsMultipartResolver的使用

需要进行springmvc.xml配置
```
<!-- SpringMVC上传文件时，需要配置MultipartResolver处理器 -->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <property name="defaultEncoding" value="utf-8" />
    <property name="maxUploadSize" value="10485760000" />
    <property name="maxInMemorySize" value="40960" />
</bean>
```

可以使用CommonsMultipartFile，也可以使用MultipartFile对象来接收文件，如果使用CommonsMultipartFile，则需要加上@RequestParam注解

- CommonsMultipartFile使用CommonsMultipartFile.getItem().write(File file)来将对象写入到其他文件中
- MultipartFile使用multipartFile.transferTo(File file)来将对象写入到其他文件中


## StandardServletMultipartResolver的使用

springmvc.xml配置
```
<!--2 注册上传  StandardServletMultipartResolver
        第二个不需要第三方 jar 包支持，它使用 servlet 内置的上传功能，
        但是只能在 Servlet 3 以上的版本使用。
        -->
<bean id="multipartResolver" 
  class="org.springframework.web.multipart.support.StandardServletMultipartResolver">

</bean>
```

web.xml
```
<servlet>
    <servlet-name>mvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:Spring_mvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>


    <multipart-config>
        <!-- 上传文件的大小限制，比如下面表示 5 M -->
        <max-file-size>5242880</max-file-size>
        <!-- 一次表单提交中文件的大小限制，必须下面代表 10 M -->
        <max-request-size>10485760</max-request-size>
        <!-- 多大的文件会被自动保存到硬盘上。0 代表所有 -->
        <file-size-threshold>0</file-size-threshold>
    </multipart-config>

</servlet>

<servlet-mapping>
    <servlet-name>mvc</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```
使用和CommonsMultipartResolver一样
