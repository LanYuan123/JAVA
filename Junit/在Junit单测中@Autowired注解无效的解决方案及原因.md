**目录：**
- [在Junit测试中@Autowired无法生效的原因](#在junit测试中autowired无法生效的原因)
- [解决方案](#解决方案)
  - [@RunWith](#runwith)
  - [@ContextConfiguration](#contextconfiguration)

# 在Junit测试中@Autowired无法生效的原因

在进行Junit单测的过程中，如果我们要使用Spring的注解，就需要在Spring容器环境下：
- **那么首先我们需要引入Spring-Test框架支持**
- **然后我们还要加载bean文件**

# 解决方案

- 使用@RunWith(SpringJUnit4ClassRunner.class)引入Spring-Test框架支持
- 使用@ContextConfiguration({"classpath:applicationContext.xml"})来加载bean文件

## @RunWith

作用：**指定使用的单元测试执行类**

使用@RunWith注解去改变Junit的默认执行类，并实现自己的Listener在平时的单元测试，如果不适用RunWith注解，那么Junit将会采用默认的执行类Suite执行

Junit语序用户指定其他的单元测试执行类，只需要我们的执行测试类继承org.junit.runners.BlockJUnit4ClassRunner就可以了，Spring的执行类SpringJUnit4ClassRunner就是继承了该类，我们平时使用Spring也比较多，为了能够更加方便的引用配置文件，我们单元测试就是用了Spring的执行类，所以需要加上@RunWith(SpringJUnit4ClassRunner.class)，加上之后，测试就会将在Spring容器环境下进行

## @ContextConfiguration

作用：**使用注解引入多个配置文件**

结合@RunWith(SpringJUnit4ClassRunner.class)使用，在spring容器环境下进行测试后，引入spring的bean配置文件，使得容器可以找到我们需要的bean
