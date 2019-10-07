# 目录
1. [SpringMVC的API实现](#springmvc的api实现)
2. [SpringMVC的注解实现](#springmvc的注解实现)

## SpringMVC的API实现

1. **导入SpringMVC的jar包**

   需要导入的jar包</br>
   除了Spring的包外，还需要导入</br>
   **commons-logging-1.2.1.1.jar**</br>
   **jstl-1.2.jar**</br>
   **webjar-ckeditor-standard-4.11.3.jar**
   
2. **在web.xml中配置DispatcherServlet**

   web.xml
   ```
    <?xml version="1.0" encoding="UTF-8"?>
    <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
             version="4.0">

        <servlet>
            <servlet-name>springmvc</servlet-name>
            <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
            <load-on-startup>1</load-on-startup>
        </servlet>
        <servlet-mapping>
            <servlet-name>springmvc</servlet-name>
            <url-pattern>*.do</url-pattern>
        </servlet-mapping>
    </web-app>
   ```
   
3. **在WEB-INF目录下创建并且配置springmvc-servlet.xml文件**

   **一旦DispatcherServlet初始化完成，它会查找一个名为[Servlet-Name]-servlet.xml的文件在WEB-INF文件夹中</br>
   在这里因为我们的DispatcherServlet命名叫springmvc，所以是创建springmvc-servlet.xml**

   springmvc-servlet.xml
   ```
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xsi:schemaLocation="
            http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd">


        <!--配置handlerMapping-->
        <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"></bean>

        <!--配置handlerAdapter-->
        <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"></bean>

        <!--配置渲染器，即是视图解析器-->
        <bean  class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="jspViewResolver">
            <property name = "viewClass" value="org.springframework.web.servlet.view.JstlView"></property>
            <!--结果视图的前缀-->
            <property name="prefix" value="/WEB-INF/Views/"></property>
            <!--结果视图的后缀-->
            <property name="suffix" value=".jsp"></property>
        </bean>

        <!--配置请求和处理器-->
        <bean name="/hello.do" class="controller.HelloController"></bean>
    </beans>
   ```
   
4. **创建Controller控制器**

   创建HelloController类实现Controller接口，并且覆写handleRequest方法
   ```
    /**
    * @author Administrator
    */
    public class HelloController implements Controller {
        @Override
        public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
            ModelAndView modelAndView = new ModelAndView("/user/hello");
            //封装要显示到视图中的数据
            modelAndView.addObject("msg","hello world");
            //视图名
            System.out.println("6666");
            return modelAndView;
        }

    }
   ```
5. **创建一个jsp页面**

   创建hello.jsp
   ```
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
        <title>success</title>
    </head>
    <body>
        <p>${msg}</p>
    </body>
    </html>
   ```
6. **运行项目**

## SpringMVC的注解实现

**web.xml**

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:mvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

DIspatcherServlet的三个初始化参数
参数|描述
|:--:|:--:
contextClass|实现WebApplicationContext接口的类，当前的servlet用它来创建上下文，如果这个参数没有指定，默认使用XmlWebApplicationContext
contextConfigLocation|传给上下文示例(由contextClass指定)的字符串，用来指定上下文的位置。这个字符串可以被分成多个字符串(使用逗号作为分隔符)来支持多个上下文(在多上下文的情况下，如果同一个bean被定义两次，后面一个优先)。
namespace|WebApplicationContext命名空间。默认值是[server-name]-servlet

如果使用注解开发，那么需要配置DIspatcherServlet的初始化参数contextConfigLocation,来设置需要解析的XML文档

该XML文档应该位于src中，不能在web下


**mvc.xml**

```
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <!--开启注解扫描-->
    <context:component-scan base-package="controller"/>

    <!--配置视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/Views/user/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```

**Controller类HelloController.java**

```
@Controller
public class HelloController{
    @RequestMapping("/hello")
    public ModelAndView hello(HttpServletRequest req, HttpServletResponse resp){
        ModelAndView mv = new ModelAndView("hello");
        mv.addObject("msg","hello world");
        return mv;
    }
}
```

该方法可以返回一个ModelAndView对象，同时也可以返回一个字符串，SpringMVC会自动将字符串拼接在URL地址上

**jsp页面hello.jsp**

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>success</title>
</head>
<body>
    <p>${msg}</p>
</body>
</html>
```
