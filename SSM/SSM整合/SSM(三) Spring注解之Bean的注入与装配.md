# Spring与Bean注入和装配相关的注解详解

## bean的注入

**当使用了SpringBean注入的注解后，我们需要在配置文件中添加<context:component-scan/>来扫描添加了注解的类，这样子声明注解的类才能起作用**

注解|
:--:|
@Component|
@Repository|
@Service|
@Controller|
@Bean|

### @Component

作用：**标注此类是一个普通的Bean，并注入到Spring框架中进行管理**

- @Component主要用于将一个Java类注入到Spring框架中，它相当于XML配置文件中的< bean id=”xxx” class=”xxx”/ >
- Bean实例的名称默认是Bean类的首字母小写，其他部分不变
- 也可以通过设置注解的value属性来自主设定bean的名称

### @Repository

作用：**标注此类是一个Dao组件类，并注入到Spring框架中进行管理**

- 与@Component的用法相同，只是标注的类表示的是Dao层组件

### @Service

作用：**标注此类是一个Service组件类，并注入到Spring框架中进行管理**

- 与@Component的用法相同，只是标注的类表示的是Service层组件

### @Controller

作用：**标注此类是一个Controller组件类，并注入到Spring框架中进行管理**

- 与@Component的用法相同，只是标注的类表示的是Controller层组件

### @Bean


### @Bean和@Component


## bean的装配

注解|
:--:|
@Autowired|
@Resource|
@Inject|

### @Autowired


### @Resource


### @Inject


### @Autowired和@Resource的区别


> 参考文章 [http://www.voidcn.com/article/p-vnuuxhnq-bcq.html](http://www.voidcn.com/article/p-vnuuxhnq-bcq.html)
