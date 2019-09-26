# bean的作用域

bean标签有一个scope属性，来设置bean的作用域

```
<bean scope = "singleton">
```

bean的作用域

属性|作用
|:--:|:--:
singleton|单例，整个容器中只有一个对象实例，默认是单例
prototype|原型，每次获取bean都产生一个新的对象
request|每次请求时创建一个新的对象
session|在会话的范围内是一个对象
global session|只在portlet下有用，表示是application
application|在应用范围中一个对象

# bean的自动装配 -- 简化Spring的配置文件

在配置bean时，可以配置bean的autowire属性，用于指定装配类型

```
<bean id ="user" class="bean.User" autowire="byName" >
</bean>
```

属性|作用
|:--:|:--:
no|不使用自动装配
byName|根据名称(set方法名来的)去查找相应的bean，如果有则装配上
byType|根据类型进行自动装配，不用管id，但是同类型的bean只能有一个
constructor|当通过构造器注入，实例化bean时，使用byType的方式，装配构造方法

可以配置全局的自动装配类型，在头部使用default-autowire

```
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd"
       default-autowire="byName"
>

</beans>
```

推荐不使用自动装配，而使用注解
