## 目录
1. [AOP(面向切面编程)](aop(面向切面编程))
2. [Spring_AOP原理](#spring-aop原理)
3. [AOP的实现者](#aop的实现者)
4. [AOP的一些术语](#aop的一些术语)
5. [Spring对AOP的支持](#spring对aop的支持)

:one:
## AOP(面向切面编程)

AOP被称为面向切面编程，那么我们怎么理解面向切面编程呢？？

我们可以看下下面这一段代码：

```
public class User {

    public void add(){
        log();
        System.out.println("add");
    }

    public void delete(){
        log();
        System.out.println("delete");
    }

    public void update(){
        log();
        System.out.println("update");
    }

    public void search(){
        log();
        System.out.println("search");
    }

    public void log(){
        System.out.println("日志");
    }
}
```

我们可以看到在User类中，log()方法一直重复，并且是非业务代码

我们以前在学Java的时候，如果代码重复了怎么办?可以分为以下几个步骤：

- 抽取成方法
- 抽取类

抽取成类的方法我们称之为：**纵向抽取**

- 通过继承的方式实现纵向抽取

但是，我们现在的办法不行，即使抽取成类还是会出现重复的代码，因为这些逻辑(开始、结束、调教事务)**依附在我们业务类的方法逻辑中**

现在纵向抽取的方式不行了，AOP的理念：就是将**分散在各个业务逻辑代码中的相同代码通过横向切割的方式**抽取到一个独立的模块中

![横向抽取](https://github.com/Lany-Java/Java/blob/master/img/%E6%A8%AA%E5%90%91%E6%8A%BD%E5%8F%96.png)

上图也很清晰了，将重复性的逻辑代码横切出来其实很容易(我们简单可认为就是封装成一个类就好了)，
但是我们要将这些被我们横切出来的逻辑代码融合到业务逻辑中，来完成和没抽取前一样的功能！这就是AOP首要解决的问题了!

---

:two:
## Spring AOP原理
    将这些被我们横切出来的逻辑代码融合到业务逻辑中，来完成和没抽取前一样的功能！
    
在没有学习Spring AOP之前，我们就可以使用代理来完成
- 代理可以干嘛？代理可以帮我们增强对象的行为！使用动态代理实质上就是调用时，拦截对象方法，对方法进行改造、增强!

在Java中动态代理有两种方式：
- JDK动态代理
- CGLib动态代理

![AOPProxy](https://github.com/Lany-Java/Java/blob/master/img/AOPproxy.png)

JDK动态代理是需要实现某个接口了，而我们类未必全部会有接口，于是CGLib代理就有了
- CGLib代理其生成的动态代理对象是目标类的自雷
- Spring AOP 默认时使用JDK动态代理，如果代理的类没有接口则会使用CGLib代理

那么JDK代理和CGLib代理我们该用哪个呢？
- 如果是单例的，我们最好使用CGLib
- 如果是多例的，我们最好使用JDK代理

原因：
- JDK在创建代理对象时的性能要高于CGLib代理，而生成代理对象的运行性能却比CGLib的低

看到这里我们应该得出了AOP的定义：**将相同逻辑的重复代码横向抽取出来，使用动态代理技术将这些重复代码织入到目标对象方法中，实现和原来一样的功能。**

这样我们就在写业务时只关心业务代码，而不用关心与业务无关的代码

---

:three:
## AOP的实现者

AOP除了有Spring AOP实现外，还有著名的AOP实现者：AspectJ，也有可能大家没听说过的实现者：JBoss AOP

```
AspectJ是语言级别的AOP实现，扩展了Java语言，定义了AOP语法，能够在编译器提供横切代码的织入，
所以它有专门的编译器用来生成遵守Java字节码规范的Class文件。
```

而Spring借鉴了AspectJ很多非常有用的做法，融合了AspectJ实现AOP的功能。但Spring AOP本质上**底层还是动态代理**，所以Spring AOP是不需要有专门的编辑器的

---

:four:
## AOP的一些术语

- **连接点(Join Point)**

    **能够被拦截的地方**：Spring AOP是基于动态代理的，所以是方法拦截的。每个成员方法都可以称之为连接点。
    
- **切点(Poincut)**

    **集体定位的连接点**：具体定位到某一个方法就称为切点
    
- **增强/通知(Advice)**

    表示添加到其代码的一切逻辑代码，并且定位连接点的方位信息
       - 简单来说就是定义了是干什么，具体是在哪里杆
       - Spring AOP提供了5中Advice类型给我们：**前置、后置、返回、异常、环绕**给我们使用
    
- **织入(Weaving)**

    将增强/通知添加到目标类的具体连接点上的过程
    
- **引入/引介(introduction)**

    允许我们向现有的类添加新方法或属性。是一种特殊的通知!
    
- **切面(Aspect)**

    切面由**切点**和**增强/通知**组成，它既包括了横切逻辑的定义，也包括了连接点的定义
    
---

:five:
## Spring对AOP的支持

Spring提供3种类型的AOP支持：
- 基于代理的经典SpringAOP
   - 需要实现接口，手动创建代理
- 纯POJO切面
  - 使用XML配置，AOP命名空间
- 注解驱动的切面
  - 使用注解的方式，这是最简洁和最方便的！
