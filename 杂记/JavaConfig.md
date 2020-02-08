**目录：**
 - [一种新型的配置方式JavaConfig](#一种新型的配置方式javaconfig)
  - [JavaConfig简介](#javaconfig简介)
  - [JavaConfig使用简介](#javaconfig使用简介)

# 一种新型的配置方式JavaConfig

## JavaConfig简介

在软件开发中，我们常常会尽力的将代码做到"高内聚、低耦合"，前辈们为了将解耦做到极致，把“什么时候需要什么样的实现”这个问题的解决方案，写在配置文件（XML）中，因为XML的改变不需要重新编译，因此达成了彻底解耦的目标。我需要什么实现，就直接在配置文件中修改即可，不需要修改代码，不需要重新编译。不得不说为了实现"彻底的解耦"，利用XML作为配置文件确实是一个非常好的想法：
- XML可以轻松的实现配置集中化，这样做不会将不同组件分散的到处都是，你可以在一个地方看到所有Bean的概况和他们的装配关系
- 如果你需要分割配置文件，Spring可以在运行时通过<import>标签或者上Context文件对分割文件进行重新聚合
- 相对于自动装配，XML配置允许显式装配
- XML配置完全和JAVA文件解耦：两种文件完全没有耦合关系，这样的话，类可以被用作多个不同XML配置文件

然而随着时间的发展和XML的应用，人们发现了以下一些问题：
- 大部分时候我们并不需要解耦的如此的彻底，应用级程序甚至很少会遇到需要更换接口的实现
- XML的学习门槛很高，有时一个老手看配置文件经常也能看的云里雾里，再加上xml文件不能做类型检查，有时候你实际上是把一个错误的类装配进接口，导致系统报错，你却要找半天错误，即我们只有在运行时环境时你才能发现各种配置及语法错误

因此近几年我们有了新的口号，即"约定大于配置"，而JavaConfig即使约定大于配置的产物（也因为Java EE5.0引入了一个非常重要的特性--annotation），JavaConfig使用Java代码+注解，它做的事情和XML配置文件几乎相同，但是因为有类型检查而且本身就是Java代码，所以它的可读性和可维护性远远高于XML配置，新手接替项目的门槛大大降低，而代价不过是放弃不知道哪天才可能用上的"彻底解耦"的能力，所以JavaConfig理所因当的正在慢慢替代XML配置

在Spring中，我们也可以明显的看到这样的趋势，在Spring 3.x之前，大家都是使用的XML文件进行配置，甚至早在Spring 2.5之前很多人就对XML配置叫苦不迭，于是从Spring 3.x开始，Spring提供JavaConfig的支持，这一特性使得Spring能够摆脱XML配置文件，在Spring4.x之后，JavaConfig就作为Spring推荐的配置方式了


## JavaConfig使用简介

在使用JavaConfig之前，需要向web.xml文件中加入：

```
<context-param>  
    <param-name>contextClass</param-name>  
    <param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext</param-value>  
</context-param>  
<context-param>  
    <param-name>contextConfigLocation</param-name>  
    <param-value>com.packtpub.learnvaadin.springintegration.SpringIntegrationConfiguration</param-value>  
</context-param> 
```

- spring ioc的bean
  
  XML配置方式中的XML文件：
  
  ```
   <?xml version="1.0" encoding="UTF-8"?>   
    <beans xmlns="http://www.springframework.org/schema/beans"  
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
        xsi:schemaLocation="http://www.springframework.org/schema/beans   
    http://www.springframework.org/schema/beans/spring-beans-3.2.xsd">   
    
  //这里就是往容器中生成一个id为button的bean 当然也可以打开注解扫描使用@Service、@Component等注解来配置
        <bean id="button" class="javax.swing.JButton">   
            <constructor-arg value="Hello World" />   
        </bean>       
    </beans>  
    
  ```
  
  JavaConfig配置方式的java类：
  
  ```
  //这个注解表明这是配置类 相当于spring配置文件
  @Configuration  
  public class MigratedConfiguration {  
  //这个注解表示注册一个bean对象 注解name值即它的id 方法返回值就是bean对象  如果注解没有name值则方法名就是id
      @Bean  
      public JButton button() {  
          return new JButton("Hello World");  
      }  
  }  
  ```

- 多配置文件加载

  XML方式：使用<import>标签加载另一个配置文件
  
  JavaConfig方式：
  
  ```
  //在配置类中使用@Import注释引入另一个配置类当然下面的CustomerConfig.class也得是配置类
  @Configuration
  @Import({ CustomerConfig.class, SchedulerConfig.class })
  public class AppConfig {

  }
  ```

- 扫描注解bean

  XML方式：
  
  ```
  //开启annotation
  <context:annotation-config/>
  //扫描这个包下的annotation
  <context:component-scan base-package="com.test"/>
  ```
  
  JavaConfig方式：
  
  ```
  //这里就包含了<context:annotation-config/>的功能
  @ComponentScan(basePackages={"com.test"})
  @Configuration
  @Import({ CustomerConfig.class, SchedulerConfig.class })
  public class AppConfig {

  }
  ```

> 参考文章：[什么是JavaConfig](https://blog.csdn.net/WuLex/article/details/78278882)</br>
> 参考文章：[Spring4.x推荐使用java配置，为什么推荐这种配置方式？与xml配置和注解配置相比有什么优势？ - 有铭的回答 - 知乎](https://www.zhihu.com/question/278435266/answer/400391692)</br>
> 参考文章：[javaConfig简介](https://blog.csdn.net/lolichan/article/details/84924322)
