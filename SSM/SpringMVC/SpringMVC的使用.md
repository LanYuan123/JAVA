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


