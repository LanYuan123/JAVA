**目录**
- [SSM整合配置](#ssm整合配置)
  * [整合思路](#整合思路)
  * [4个配置文件的配置](#4个配置文件的配置)
    + [applicationContext.xml（Spring的配置文件）](#applicationcontextxmlspring的配置文件)
      - [context:property-placeholder location=""](#contextpropertyplaceholder-location)
      - [context:component-scan base-package=""](#contextcomponentscan-basepackage)
      - [context:annotation-config](#contextannotationconfig)
      - [bean id="datasource" class="com.alibaba.druid.pool.DruidDataSource"](#bean-iddatasource-classcomalibabadruidpooldruiddatasource)
      - [bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean"](#bean-idsqlsessionfactory-classorgmybatisspringsqlsessionfactorybean)
      - [bean id="sqlSessionTemplate" class="org.mybatis.spring.SqlSessionTemplate"](#bean-idsqlsessiontemplate-classorgmybatisspringsqlsessiontemplate)
    + [springmvc.xml（SpringMVC的配置文件）](#springmvcxmlspringmvc的配置文件)
      - [mvc:annotation-driven](#mvcannotationdriven)
      - [bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"](#bean-idmultipartresolver-classorgspringframeworkwebmultipartcommonscommonsmultipartresolver)
    + [SqlMapConfig.xml（Mybatis的配置文件）](#sqlmapconfigxmlmybatis的配置文件)
      - [settings](#settings)
      - [typeAliases](#typealiases)
      - [mappers](#mappers)
    + [web.xml](#webxml)
      - [context-param](#context-param)
      - [listener](#listener)
      - [filter](#filter)
      - [servlet](#servlet)


# SSM整合配置

## 整合思路

Spring整合Mybatis，Mybatis将以下事情交由Spring管理：</br>
1. 读入数据库连接相关参数，配置数据连接池
2. 通过Spring管理SqlSessionFactory、mapper代理对象
3. Spring来管理事务
4. 配置扫描Dao接口，动态实现Dao接口，并注入到Spring容器中
5. Service层所有实现类全部放到Spring容器中进行管理

SpringMVC整合Spring，Controller层交给SpringMVC来管理

## 4个配置文件的配置

### applicationContext.xml（Spring的配置文件）

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <!--开启该配置，隐式的向Spring容器中注册了
    AutowiredAnnotationBeanPostProcessor,
    CommonAnnotationBeanPostProcessor,
    PersistenceAnnotationBeanPostProcessor,
    RequiredAnnotationBeanPostProcessor
    这4个BeanPostProcessor
    注册这4个bean处理器主要的作用是为了你的系统能够识别相应的注解
    比如AutowiredAnnotationBeanPostProcessor就是识别@AutoWired
    而@AutoWired 默认按类型匹配
-->
    <context:annotation-config/>

    <!-- 1. 用于加载用于数据库配置的属性文件 -->
    <context:property-placeholder location="classpath:properties/db.properties"/>

    <!-- 2. 用于进行service和dao包扫描 -->
    <context:component-scan base-package="controller,dao,service"/>

    <!-- 3. datasource 数据源配置，使用的连接池 -->
    <bean id="datasource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init"
          destroy-method="close">
        <!--        驱动          -->
        <property name="driverClassName" value="${jdbc.driver}"/>
        <!--        url          -->
        <property name="url" value="${jdbc.url}"/>
        <!--        用户名        -->
        <property name="username" value="${jdbc.username}"/>
        <!--        密码          -->
        <property name="password" value="${jdbc.password}"/>
        <!--        连接池中保留的最大连接数，默认为15        -->
        <property name="maxActive" value="${c3p0.pool.maxPoolSize}"/>
        <!--        连接池中保留的最小连接数，默认为15        -->
        <property name="minIdle" value="${c3p0.pool.minPoolSize}"/>
        <!--        初始化时创建的连接数，应该在minPoolSize与maxPoolSize之间取值        -->
        <property name="initialSize" value="${c3p0.pool.initialPoolSize}"/>
    </bean>

    <!-- 4. 配置sqlSessionFactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="datasource"/>
        <property name="configLocation" value="config/SqlMapConfig.xml"/>
    </bean>

    <!-- 5. 配置sqlSessionTemplate -->
    <bean id="sqlSessionTemplate" class="org.mybatis.spring.SqlSessionTemplate">
        <constructor-arg index="0" ref="sqlSessionFactory"/>
    </bean>

</beans>
```

配置说明：</br>

#### context:property-placeholder location=""

```
引入外部properties配置文件，可以通过${}取值
```

#### context:component-scan base-package=""

```
spring会去自动扫描base-package对应的路径或者该路径的子包下面的java文件，
如果扫描到文件中带有@Service,@Component,@Repository,@Controller等这些注解的类，则把这些类注册为bean。
如果配置了<context:component-scan>那么<context:annotation-config/>标签就可以不用在xml中再配置了，因为前者包含了后者
```

#### context:annotation-config

```
开启该配置，隐式的向Spring容器中注册了AutowiredAnnotationBeanPostProcessor,
CommonAnnotationBeanPostProcessor,PersistenceAnnotationBeanPostProcessor,
RequiredAnnotationBeanPostProcessor这4个BeanPostProcessor,
注册这4个bean处理器主要的作用是为了你的系统能够识别相应的注解,
比如AutowiredAnnotationBeanPostProcessor就是识别@AutoWired
```

#### bean id="datasource" class="com.alibaba.druid.pool.DruidDataSource"

```
该类是数据库datasource数据源的配置
需要配置的参数：
数据库连接参数配置，数据库驱动的加载
还有一些选择项，看情况配置
```

#### bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean"

```
本来是Mybatis的获取sqlSession的工厂类，现在交给Spring管理
但是在Mybatis和Spring整合之后，我们不再使用sqlSessionFactory，而是使用整合包提供的sqlSessionTemplate
该类需要注入数据源的类和配置文件位置
```

#### bean id="sqlSessionTemplate" class="org.mybatis.spring.SqlSessionTemplate"

```
需要以sqlSessionFactory作为构造方法参数来创建sqlSessionTemplate对象
需要注入一个sqlSessionFactory类
在整合之后使用此类来代替sqlSessionFactory使用
```
       
### springmvc.xml（SpringMVC的配置文件）

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--    扫描Controller    -->
    <context:component-scan base-package="controller"/>

    <!--    注解驱动    -->
    <mvc:annotation-driven/>

    <!-- 6. 配置文件上传CommonsMultipartResolver类 -->
    <bean id="multipartResolver"
          class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <!-- 文件编码格式 -->
        <property name="defaultEncoding" value="UTF-8"/>
        <!-- 最大上传文件大小 -->
        <property name="maxUploadSize" value="104857600000"/>
        <!-- 内存中的最大值 -->
        <property name="maxInMemorySize" value="40960" />
    </bean>

    <!-- 使用ResponseBody注解，不会再走视图解析器，所以不再需要配置视图解析器 -->
</beans>
```

配置说明：</br>

#### mvc:annotation-driven

```
简单来说这个配置可以让我们在Controller层中使用的各种注解生效起作用
```

#### bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"

```
文件上传需要的类，可以通过注入多个参数，来确定文件编码格式，最大上传文件大小等
```

### SqlMapConfig.xml（Mybatis的配置文件）

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    <!-- 全局配置 -->
    <settings>
        <setting name="useGeneratedKeys" value="true"/>
    </settings>
    <!-- 别名 -->
    <typeAliases>
        <package name="entity"/>
    </typeAliases>
    <!-- 非注解实现，注解实现可不用此项配置 -->
    <mappers>
        <mapper resource="mapper/ClassMapper.xml"/>
        <mapper resource="mapper/ClassroomMapper.xml"/>
        <mapper resource="mapper/CourseMapper.xml"/>
        <mapper resource="mapper/TermMapper.xml"/>
        <mapper resource="mapper/UserMapper.xml"/>
        <mapper resource="mapper/ClassRoomTypeMapper.xml"/>
    </mappers>
</configuration>
```

#### settings

```
一些全局配置，比如配置缓存、延迟加载、结果集控制、分页设置等，都在这里配置
```

#### typeAliases

```
别名配置
```

#### mappers

```
指明Mapper配置文件的位置，如果使用注解则不需要此项配置
```


### web.xml

```
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
         http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">

  <display-name>Archetype Created Web Application</display-name>

  <!--配置spring-->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:config/applicationContext.xml</param-value>
  </context-param>
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>

  <!--配置springMvc-->
  <!--配置过滤器使得springMvc可以接收来自put和delete的请求-->
  <filter>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
  </filter>
  <filter-mapping>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>

  <!--配置dispatcherServlet-->
  <servlet>
    <servlet-name>dispatchServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:config/springmvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>dispatchServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>

</web-app>

```

配置说明：</br>

#### context-param

```
指明Spring配置文件的位置，在Web启动的时候，供其加载
```

#### listener

```
在Spring的整合项目中一般都要加入
就是上面这段配置为项目提供了spring支持，初始化了Ioc容器
```

#### filter

```
使得SpringMVC可以接收PUT和DELETE请求
```

#### servlet

```
配置SpringMVC最核心的dispatcherServlet，作为转发站和中央处理器
```

