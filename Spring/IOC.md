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
