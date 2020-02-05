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

    <!-- 使用ResponseBody注解，不在需要视图解析器 -->
</beans>
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

### web.xml
