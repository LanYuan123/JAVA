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
  
  前置通知将会在连接点被调用
  
  在前置通知类中的before方法中调用被织入了通知的方法时，该方法不会被拦截，在其他通知中也同理
  
  以下是一个例子：
  
  **前置通知类Log.java**
  ```
  public interface MethodBeforeAdvice extends BeforeAdvice {

      void before(Method m, Object[] args, Object target) throws Throwable;

  }
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
  
  标签|作用
  |:--:|:--:
  <aop:config> </aop:config>|总的AOP的配置标签
  <aop:pointcut id="pointcut" expression="execution(* service.UserServiceImpl.add())"/>|配置通知类 id为该通知的唯一标识符，express告诉Spring这个通知将会被织入到哪些方法上，在里面固定的填写execution(),在execution()中填写返回值和需要织入通知的位置，*()表示的是所有方法，而方法名(..)表示所有不同参数的该方法
  <aop:advisor advice-ref="log" pointcut-ref="pointcut"></aop:advisor>|将通知类织入方法，advice-ref表示切点(即目标类)，pointcut-ref表示通知类
  
  
### 2. 后置通知

与前置通知大致相同

只是后置通知是实现AfterReturningAdvice接口，并且覆写afterReturning方法
```
public interface AfterReturningAdvice extends Advice {

    void afterReturning(Object returnValue, Method m, Object[] args,
            Object target) throws Throwable;

}

public class Afterlog implements AfterReturningAdvice {
    @Override
    public void afterReturning(Object o, Method method, Object[] objects, Object o1) throws Throwable {
        System.out.println(
                o1.getClass().getName()+"的"+method.getName()+"方法执行完成"
        );
    }
}
```

afterReturning方法中的几个参数所代表的意思：

参数|含义
|:--:|:--:
第一个Object o|被调用方法的返回值（不能修改）
Method method|被调用方法对象
Object[] objects|方法参数
第二个Object o1|调用方法的目标对象


### 3. 异常通知

如果一个RemoteException（包括子类）被抛出，异常通知就会被调用

与前置通知大致相同,不过异常通知是实现ThrowsAdvice接口，并且覆写afterThrowing方法

```
public class RemoteThrowsAdvice implements ThrowsAdvice {

    public void afterThrowing(Method m, Object[] args, Object target, ServletException ex) throws Throwable {
        // Do something with remote exception
    }

}
```

**注意：**

1. 在afterThrowing方法中有四个参数，只有最后一个参数ServletException ex是必要的，其他的三个参数可选，所以这个方法可能拥有一个或者四个参数

2. 如果一个异常抛出增强本身抛出了一个异常，它将覆盖掉原始的异常（例如，改变抛给用户的异常），这个覆盖的异常通常是一个运行时异常；这样就可以兼容任何的方法签名。 但是，如果一个异常抛出增强抛出了一个检查时异常，这个异常必须和该目标方法的声明匹配，以此在一定程度上与特定的目标签名相结合。

3. 不要抛出与目标方法签名不兼容的检查时异常！

参数|含义
|:--:|:--:
Method m|被调用的方法对象
Object[] args|方法参数
Object target|调用方法的目标对象
ServletException ex|抛出的异常对象


### 4. 拦截式环绕通知

环绕通知需要实现MethodInterceptor接口，并且覆写invoke方法
```
public interface MethodInterceptor extends Interceptor {

    Object invoke(MethodInvocation invocation) throws Throwable;

}

public class DebugInterceptor implements MethodInterceptor {

    public Object invoke(MethodInvocation invocation) throws Throwable {
        System.out.println("Before: invocation=[" + invocation + "]");
        Object rval = invocation.proceed();
        System.out.println("Invocation returned");
        return rval;
    }

}
```

invoke() 方法的MethodInvocation表示了将要被调用的方法、目标连接点、AOP代理、以及该方法的参数。 invoke() 方法应当返回调用结果：目标连接点的返回值。

注意调用MethodInvocation对象的proceed()方法。这个方法将拦截器链路调向连接点。 大多数拦截器会调用该方法，并返回该方法的值。但是，就像任意环绕通知一样，一个方法拦截器也可以返回一个不同的值，或者抛出一个异常，而不是调用proceed()方法。 但是，没有足够的理由，不要这么干！

### 5. 引介通知

Spring将引介通知当作一个特殊的拦截式通知。

引介增强需要一个IntroductionAdvisor和一个IntroductionInterceptor实现以下接口：
```
public interface IntroductionInterceptor extends MethodInterceptor {

    boolean implementsInterface(Class intf);

}

public interface IntroductionAdvisor extends Advisor, IntroductionInfo {

    ClassFilter getClassFilter();

    void validateInterfaces() throws IllegalArgumentException;

}

```



## AOP的自定义类实现

## AOP的注解实现
