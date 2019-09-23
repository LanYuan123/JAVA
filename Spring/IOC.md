# Spring ---- 一个开源的轻量级的JavaSE/JavaEE开发应用框架
    优点：
      1. 轻量级框架
      2. IOC容器---控制反转
      3. AOP面向切面编程
      4. 对事务的支持
      
## IOC -- inversion fo control 控制反转
    控制是对创建对象的控制
    
    反转是对象由比原来程序本身创建，变成了程序接受对象
    
    这样实现了service和dao的解耦工作。service层和dao层实现了分离。没有直接依赖关系
    即使dao的实现发生了改变，应用程序本身不用改变


一个示例：

**Hello.java**

```
package bean;

public class Hello {
    private String name;
    public void setName(String name){
        this.name = name;
    }
    public void show(){
        System.out.println("hello"+ name);
    }
}
```

**beean.xml**

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--bean就是java对象 由spring来创建和管理-->
    <bean name="hello" class="bean.Hello">
        <property name="name" value="张三"/>
    </bean>
</beans>
```

**test.java**

```
package Test;

import bean.Hello;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class test {
    public static void main(String[] args) {
        //解析beans.xml文件  生成相应的bean对象
        ApplicationContext context =new ClassPathXmlApplicationContext("beans.xml");
        Hello hello = (Hello)context.getBean("hello");
        hello.show();
    }
}

```

在这个过程中，Hello对象是由谁来创建的？Hello对象属性是由谁来设置的？

Hello对象由Spring容器创建，同时对象属性也由Spring容器来设置

这样一个过程就叫控制反转

---
**控制**：指谁来控制对象的创建，传统的应用程序对象的创建是由程序本身控制的。</br>
在使用Spring之后，是由Spring来创建的对象的。

**反转**：一般是程序来创建对象，反转指程序本身不去创建对象，而变为被动接收的对象，

**总结：**</br>
    1. **控制反转，以前对象是程序本身来创建，使用Spring后，程序为被动接收Spring创建好的对象**</br>
    2. **IOC是一种编程思想，不是一种技术或者设计模式**</br>
    3. **IOC的实现是通过IOC容器来实现的,IOC容器——BeanFactory**</br>
    
----

## IOC创建对象的三种方式

### :sunny: 通过无参的构造方法来创建

   User..java
   ```
   public class User {
    public User(){
        System.out.println("这是无参构造！");
    }
    private String name;

    public void setName(String name){
        this.name = name;
    }
    public void show(){
        System.out.println("hello"+ name);
    }
}
   ```
   beans.xml
   ```
   <bean id="user" class="bean.User">
        <property name="name" value="张三"/>
    </bean>
   ```
### :sunny: 通过有参的构造方法来创建

   - **通过参数的下标来设置**
   
       User.java
       ```
       public class User {
          public User(String name){
              this.name = name;
              System.out.println("这是有参构造！");
          }
          private String name;

          public void setName(String name){
              this.name = name;
          }
          public void show(){
              System.out.println("hello"+ name);
          }
       }
       ```
       beans.xml
       ```
       <bean id ="user" class="bean.User">
        <!--index指构造方法，参数下标从0开始-->
        <constructor-arg index="0" value="李四"></constructor-arg>
       </bean>
       ```
       
       index代表构造方法的第几个参数
       
       如果有多个参数则使用多个constructor-arg标签
       
   - **通过参数名称来设置**
   
       User.java
       ```
       public class User {
          public User(String name){
              this.name = name;
              System.out.println("这是有参构造！");
          }
          private String name;

          public void setName(String name){
              this.name = name;
          }
          public void show(){
              System.out.println("hello"+ name);
          }
       }
       ```
       beans.xml
       ```
       <bean id ="user" class="bean.User">
        <!--name指参数名-->
        <constructor-arg name="name" value="李四"></constructor-arg>
        </bean>
       ```
       
       name指参数名，如果有多个参数，则使用多个constructor-arg标签
       
   - **通过参数的类型**
   
       User.java
       ```
       public class User {
         public User(String address, String name) {
             this.address = address;
             this.name = name;
         }

         private  String address;
         private String name;
 
         public void setAddress(String address) {
             this.address = address;
         }
 
         public void setName(String name){
             this.name = name;
         }
         public void show(){
             System.out.println("hello"+ name);
         }
       }
       ```
       beans.xml
       ```
       <bean id ="user" class="bean.User">
        <!--type指定参数类型-->
        <constructor-arg type="java.lang.String" value="李四"></constructor-arg>
        <constructor-arg type="java.lang.String" value="成都"></constructor-arg>
        </bean>
       ```
       
       如果多个参数的类型都相同，那么就按照constructor-arg标签的顺序来
       
### :sunny: 通过工厂方法来创建
   - **静态工厂**
   
        UserFactory.java
        ```
        public class UserFactory {
            public static User newInstance(String name){
                return new User(name);
            }
        }
        ```
        beans.xml
        ```
        <bean id ="user" class="factory.UserFactory" factory-method="newInstance"> 
            <constructor-arg type="java.lang.String" value="李四"></constructor-arg>
        </bean>
        ```
        
        class的值是工厂类的包名加类名，factory-method的是要调用的工厂类中的静态方法名
        
        如果有多个参数，则使用多个constructor-arg标签
        
        在bean标签中可以使用无参构造<property>标签，也可以使用三种有参构造<constructor-arg>标签
        
   - **动态工厂**
        
        UserFactory.java
        ```
        public class UserFactory {
            public User newInstance(){
                return new User();
            }
        }
        ```
        beans.xml
        ```
        <bean id="userFactory" class="factory.UserFactory"></bean>
        <bean id ="user" factory-method="newInstance" factory-bean="userFactory">
             <!--type指定参数类型-->
            <property name="name" value="lisi"></property>
        </bean>
        ```
        
        动态工厂相较于静态工厂，工厂里面的方法不在是静态的了
        
        要用bean标签引入工厂类
        
        同时使用factory-bean代替class，factory-bean的值是工厂类的包名加类名
        

        
        
