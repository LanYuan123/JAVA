## 目录
1. [AOP的API实现](#aop的api实现)
    - [前置通知](#1-前置通知)
    - [后置通知](#2-后置通知)
    - [异常通知](#3-异常通知)
    - [环绕通知](#4-环绕通知)
    - [引介通知](#5-引介通知)
2. [AOP的自定义类实现](#aop的自定义类实现)
3. [AOP的注解实现](#aop的注解实现)

## AOP的API实现

### 1. 前置通知

  前置通知需要我们自定义一个类去实现BeforeAdvice接口，并且在实现了接口之后，覆写其中的before方法
  
  前置通知将会在切点被调用之前被调用
  
  在前置通知类中的before方法中调用目标类的方法时，不会对该方法进行方法拦截 
  
  以下是一个例子：
  
  **前置通知类Log.java**
  ```
  public class log implements MethodBeforeAdvice {
      @Override
      public void before(Method method, Object[] objects, Object o) throws Throwable {
          System.out.println(o.getClass().getName()  +" 的 "+ method.getName());
      }
  }
  ```
  
  注意：before方法中的几个参数解释
  
  参数|作用
  |:--:|:--:
  method|被调用方法的对象
  objects|别调用方法的参数
  o|被调用方法的目标对象
      
  
  **目标类UserServiceImpl.java**
  ```
  public interface UserService {
      void add();
      void delete();
      void update();
      void search();
  }
  
  public class UserServiceImpl implements UserService{
      @Override
      public void add() {
          System.out.println("执行add方法!");
      }

      @Override
      public void delete() {
          System.out.println("执行delete方法!");
      }

      @Override
      public void update() {
          System.out.println("执行update方法!");
      }

      @Override
      public void search() {
          System.out.println("执行search方法!");
      }
  }
  ```
  
  **测试类Test.java**
  ```
  public class Test {
      public static void main(String[] args) {
          ApplicationContext applicationContext =
                  new ClassPathXmlApplicationContext("beans.xml");
          UserService userService =
                  (UserService) applicationContext.getBean("userService");
          userService.add();
      }
  }
  ```
  
  **AOP的XML配置文件**
  ```
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:aop="http://www.springframework.org/schema/aop"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd
         http://www.springframework.org/schema/aop
         http://www.springframework.org/schema/aop/spring-aop.xsd
          "
  >
      <bean id="userService" class="service.UserServiceImpl"></bean>
      <bean id="log" class="Log.log"></bean>
      <bean id="Afterlog" class="Log.Afterlog"></bean>
      <aop:config>
          <aop:pointcut id="pointcut" expression="execution(* service.UserServiceImpl.add())"/>
          <aop:advisor advice-ref="log" pointcut-ref="pointcut"></aop:advisor>
          <aop:advisor advice-ref="Afterlog" pointcut-ref="pointcut"></aop:advisor>
      </aop:config>
  </beans>
  ```
  
### 2. 后置通知

### 3. 异常通知

### 4. 环绕通知

### 5. 引介通知


## AOP的自定义类实现

## AOP的注解实现
