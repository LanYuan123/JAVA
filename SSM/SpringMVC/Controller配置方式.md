## Controller配置方式

### 1. 通过URL对应Bean

使用此类配置方式，需要在XML中做出如下配置
```
<!--配置handlerMapping-->
<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"></bean>

<!--配置handlerAdapter-->
<bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"></bean>

<!--配置请求和处理器-->
<bean name="/hello.do" class="controller.HelloController"></bean>
```

以上配置，访问/hello.do就会寻找ID为/hello.do的Bean，此类方式仅使用于小型的应用系统

### 2. 为URL分配Bean

使用一个统一配置几个，对各个URL对应的Controller做关系映射
```
<bean class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
    <property name="mappings">
        <props>
            <!--key对应url请求名  value对应处理器的id-->
            <prop key="/hello.do">HelloController</prop>
        </props>
    </property>
</bean>

<!--配置请求和处理器-->
<bean id="HelloController" class="controller.HelloController"></bean>
```
此类配置还可以使用通配符，访问/hello.do时，Spring会把请求分配给HelloController进行处理

### 3. URL匹配Bean

如果定义的Controller名称规范，也可以使用如下配置
```
<bean class="org.springframework.web.servlet.mvc.support.ControllerClassNameHandlerMapping"></bean>  
<bean name="helloController" class="test.HelloController"></bean>  
```

### 4. 注解

首先在配置文件中开启注解
```
<!-- 启用 spring 注解 -->  
<context:component-scan base-package="test" />  
<bean class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter"/>  
```

在编写类上使用注解@org.springframework.stereotype.Controller标记这是个Controller对象</br>
使用@RequestMapping("/hello.do")指定方法对应处理的路径

