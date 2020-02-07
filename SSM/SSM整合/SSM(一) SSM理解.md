**目录**
- [SSM框架整合的理解](#ssm框架整合的理解)
  * [Spring](#spring)
    + [Spring之IOC](#spring之ioc)
    + [Spring之AOP](#spring之aop)
  * [SpringMVC](#springmvc)
    + [SpringMVC的优点](#springmvc的优点)
    + [SpringMVC处理请求流程](#springmvc处理请求流程)
  * [Mybatis](#mybatis)
    + [传统开发模式的缺陷](#传统开发模式的缺陷)
      - [JDBC连接数据库模式分析](#jdbc连接数据库模式分析)
      - [JDBC操作SQL语句模式分析](#jdbc操作sql语句模式分析)
      - [待优化的问题](#待优化的问题)
      - [对传统开发模式问题的解决](#对传统开发模式问题的解决)
    + [Mybatis运行流程](#mybatis运行流程)


# SSM框架整合的理解

## Spring

Spring就像是整个项目中装配bean的大工厂，在配置文件中配置好参数，通过调用类的构造方法来实例化对象。</br>
Spring的核心思想是IOC（控制反转），这使得程序员不用在显式的new一个对象，而是让Spring框架帮你来完成这一切，我们只需要从Spring中获取该对象即可

### Spring之IOC

在我们的日常开发中，创建对象的操作随处可见，每次需要对象时，需要亲手将其new出来，甚至某些情况下由于坏的编程习惯还会造成对象无法回收，这是相当糟糕的。但是更为严重的是，我们一直倡导的松耦合，少侵入的原则，再这样的情况下是无法实现的。于是前辈们开始谋求改变这种编程陋习，考虑如何使用编码才能更加解耦合，由此而来的解决方案就是面向接口编程，于是就有了如下写法：
```
public class BookServiceImpl {

  //class
  private  BookDaoImpl bookDaoImpl;
  public void oldCode(){
      //原来的做法
      bookDaoImpl=new bookDaoImpl();
      bookDaoImpl.getAllCategories();
  }

 }
  //=================new====================

public class BookServiceImpl {

  //interface
  private BookDao bookDao;

  public void newCode(){
      //变为面向接口编程
      bookDao=new bookDaoImpl();
      bookDao.getAllCategories();
      }
}
```
BookServiceImpl类中由原来直接与BookDaoImp打交互变为BookDao，即使BookDao最终实现依然是BookDaoImp，这样的做的好处是显而易见的，所有调用都通过接口bookDao来完成，而接口的真正的实现者和最终的执行者就是bookDaoImpl，当替换bookDaoImpl类，也只需修改bookDao指向新的实现类。

虽然上述代码在很大程度上降低了代码的耦合度，但是代码依旧存在入侵性和一定程度的耦合性，比如在修改bookDao的实现类时，仍然需要更改BookServiceImpl的内部代码，当依赖的类多起来时，查找和修改的过程也会显得相当尴尬，因此我们仍需要寻找一种方式，他可以令开发者在无需触及BookServiceImppl代码的情况下，修改bookDao的实现类，以便达到最低的耦合度和最少的入侵性的目的。这时Java中的反射机制就可以很好地协助我们解决以上问题，反射根据给出的完整类名来动态地生成对象，这种编程方式让对象在生成时才决定到底是哪一种对象，因此可以这样假设，在某个配置文件中，已经写好bookDaoImpl类的完全限定名称，通过读取该文件而获取到bookDao的真正实现类完全限定名称，然后通过反射技术在运行时动态生成该类，最终赋值给bookDao接口，也就解决了上述问题，以下简单演示，使用properties文件作为配置文件：
```
bookDao.name=com.zejian.spring.dao.BookDaoImpl
```
获取该配置文件信息动态为bookDao生成实现类：
```
public class BookServiceImpl implements BookService
{
    //读取配置文件的工具类
    PropertiesUtil propertiesUtil=new PropertiesUtil("conf/className.properties");

    private BookDao bookDao;

    public void DaymicObject() throws ClassNotFoundException, IllegalAccessException, InstantiationException {
        //获取完全限定名称
        String className=propertiesUtil.get("bookDao.name");
        //通过反射
        Class c=Class.forName(className);
        //动态生成实例对象
        bookDao= (BookDao) c.newInstance();
    }
}
```
的确如我们所愿生成了bookDao的实例，这样做的好处是在替换bookDao实现类的情况只需修改配置文件的内容而无需触及BookServiceImpl的内部代码，从而把代码修改的过程转到配置文件中，相当于BookServiceImpl及其内部的bookDao通过配置文件与bookDao的实现类进行关联，这样BookServiceImpl与bookDao的实现类间也就实现了解耦合，当然BookServiceImpl类中存在着BookDao对象是无法避免的，毕竟这是协同工作的基础，我们只能最大程度去解耦合。

了解了以上问题我们在来理解IOC就显得简单多了，**Spring IOC就是：一个java对象，在某些特定的时间被创建后，可以进行对其他对象的控制，包括初始化、创建、销毁等。简单的理解，在上述过程中，我们通过配置文件配置了BookDaoImpl实现类的完全限定名称 ，然后利用反射在运行时为BookDao创建实际实现类，包括BookServiceImpl的创建，Spring的IOC都会帮我们完成，而我们唯一要做的就是把需要创建的类和其他类依赖的类以配置的方式告诉IOC容器需要创建和注入哪些类即可。Spring通过这种控制反转的设计模式促进了松耦合，这种方式使一个对象依赖其他对象时会通过被动的方式传送进来（如BookServiceImpl被创建时，其依赖的BookDao的实现类也会同时被注入BookServiceImpl中），而不是通过手动创建这件这些类，我们可以将IOC模式看做是工厂模式的升华版，可以吧IOC看做事一个大厂，只不过这个大工厂里面要生成的对象都是在配置文件（XML）中给出来的，然后利用Java的反射技术，根据XML中给出的类名生成相应的对象。从某种程度上来说，IOC相当于把在工厂方法里面通过硬编码创建对象的代码，改变由XML文件来定义，也就是把工厂和对象生成这两者独立分开，目的就是提高灵活性和可维护性，也达到最低的耦合度，因此我们要明白所谓的IOC就是将对象的创建权，交由Spring来完成，从此解放手动创建对象的过程，同时让类和类之间的关系到达最低的耦合度 **

### Spring之AOP

----

## SpringMVC

SpringMVC在项目中负责接收拦截用户请求，**它的核心Servlet即DispatcherServlet是一个中转站，将用户请求通过HandlerMapping分发到Controllr的各个方法上**，所以**SpringMVC是基于方法进行开发的一种框架**，Controller的各个方法对应各个请求需要执行的操作

### SpringMVC的优点

1. **SpringMVC本身是与Spring框架结合而成的**，它同时拥有Spring的优点(例如控制反转(IOC)和切面编程(AOP)等)
2. SpringMVC提供了强大的**约定大于配置的契约式编程支持**，即提供一种软件设计范式，减少软件开发人员做决定的次数，开发人员仅需规定应用中不符合约定的部分
3. 支持**灵活的URL带页面控制器的映射**
4. 可以方便于其他视图技术（比如FreeMaker等）进行整合。由于SpringMVC的模型数据往往是放置在Map数据结构中，因此其可以很方便的被其他框架引用
5. 拥有十分**简洁的异常处理机制**
6. 可以十分灵活的**实现数据验证、格式化和数据绑定机制**，可以使用任意对象进行数据绑定操作
7. **支持RESTful风格**

### SpringMVC处理请求流程

SpringMVC的处理请求流程是通过大量的组件来处理的

组件：</br>
1. **前端控制器（DispatcherServlet）**：接收用户的请求，然后给用户反馈结果，相当于一个转发器或者中央处理器，控制整个流程的执行，对各个组件进行
统一调度，降低组件之间的耦合性，利于组件之间的拓展
2. **处理器映射器（HandlerMapping）**：根据请求的URL路径，通过注解或者XML配置，寻找匹配的处理器信息
3. **处理器适配器（HandlerAdapter）**：根据映射器找到的处理器信息，按照特定规则执行相关得到处理器
4. **处理器（Hander）**：其作用是执行相关的请求处理逻辑，并且返回相应的数据和视图信息，将其封装至ModelAdnView对象中
5. **视图解析器（View Resolver）**：进行解析操作，通过ModelAndView对象中的View信息将逻辑视图名解析成真正的视图View（如通过一个JSP路径返回一个JSP页面）
6. **视图（View）**：本身是一个接口，实现类支持不同的View类型（JSP、FreeMaker、Excecl等）

![SpringMVC处理请求流程](https://github.com/Lany-Java/StudyNotes/blob/master/SSM/SSM%E6%95%B4%E5%90%88/img/SpringMVC%E8%AF%B7%E6%B1%82%E6%B5%81%E7%A8%8B.png)

请求流程如下：</br>
1. 用户单击某个请求路径，发起一个request请求，此请求会被前端控制器DispatcherServlet处理
2. 前端控制器DispatcherServlet请求 处理器映射器HandlerMapping，去查找Handler。可以依据注解或者XML配置去找
3. 处理器映射器根据配置找到相应的Handler（可能包含若干个拦截器），返回给前端控制器（DispatcherServlet）
4. 前端控制器（DispatcherServlet）请求处理器适配器（HandlerAdapter）去执行相应的Handler（常被称作Controller）
5. 处理器适配器（HandlerAdapter）执行相应的Handler
6. Handler执行完毕后会返回处理器适配器（HandlerAdapter）一个ModelAndView对象（SpringMVC底层对象，包括Model数据模型和View视图信息）
7. 处理器适配器（HandlerAdapter）接收到ModelAndView后，将其返回给前端控制器DispatcherServlet
8. 前端控制器（DispatcherServlet）接收到ModelAndView对象后，会请求视图解析器（ViewResolver）对视图进行解析
9. 视图解析器（ViewResolver）根据View信息匹配到相应的视图结果，反馈给前端控制器（ViewResolver）
10. 前端控制器（DispatcherServlet）收到View的具体视图后，进行视图渲染，将Model中的数据模型填充到View视图中的request域，生成最终视图
11. 前端控制器（DispatcherServlet）返回请求结果

----

## Mybatis

Mybatis是对对象的封装，它让数据库底层操作变得透明，Mybatis得的操作都是围绕一个sqlSessionFactory对象展开的，Mybatis通过配置文件关联到各实体类的
Mapper文件，Mapper文件中配置了每个类对数据库所需进行的sql语句映射。在每次和数据库交互时，通过sqlSessionFactory拿到一个sqlSession，在执行sql命令。

### 传统开发模式的缺陷

JDBC技术作为Java Web数据库连接核心API，已经成为Java Web开发中不可或缺的工具，但是传统的数据库连接的开发模式是有局限性的，了解其需要优化的地方，
才能更好的理解Mybatis框架的优点所在。

#### JDBC连接数据库模式分析

使用JDBC进行数据库连接时，一般都在引入相关的数据库驱动jar包后，创建一个数据库连接类，该类提供数据库驱动的加载、数据库连接参数配置，
连接对象的获取以及连接对象的关闭操作。</br>
在这种操作模式下，数据库驱动程序名称、数据库连接地址、数据库账号及密码等程序中外部变量值，都是硬编码到程序中的，当我们需要修改时，就需要修改源码并且重新编译，采用硬编码的软件项目一般来说扩展性都非常差。</br>
更重要的一点是，在每一个操作数据库的类中，都需要引入一个数据库连接类，如果获取数据库连接，等到操作完毕，然后关闭数据库连接，这样对数据库进行频繁的连接关闭操作，会造成数据库资源的浪费，十分影响数据库的性能

#### JDBC操作SQL语句模式分析

在传统JDBC开发模式下，我们在获取数据库连接对象的同时，还要创建一个预编译对象，去加载并编译SQL语句，然后使用连接对象的prepareStatement方法将编写好的
SQL语句预编译，得到预编译对象后，在传入参数，并执行SQL语句。</br>
在这样的开发模式下，如果我们要修改SQL语句和插入的参数，就要对源代码进行修改，然后重新编译，打包上线，这样也十分不利于软件系统的维护和发展

#### 待优化的问题

1. 连接参数、SQL语句的硬编码
2. 数据库的频繁连接与断开
3. 查询结果集取数据的硬编码

#### 对传统开发模式问题的解决

1. Mybatis可以将SQL语句配置在XML文件中，这就避免了JDBC在Java类中添加SQL语句的硬编码问题
2. Mybatis提供的输入参数映射方式，将参数自由灵活地配置在SQL语句配置文件中，解决了JDBC中参数在Java类中手工配置的问题
3. Mybatis通过输出映射机制，将结果集的检索自动映射成响应的Java对象，避免了JDBC对结果集的手工检索
4. Mybatis还可以创建自己的数据库连接池，使用XML配置文件的形式，对数据库连接数据进行管理，便面了JDBC的数据库连接参数的硬编码问题

### Mybatis运行流程

Mybatis的整个运行流程，是紧紧围绕着数据库连接词配置文件SqlMapConfig.xml，以及SQL映射配置文件Mapper.xml而展开的。

![Mybatis的执行流程](https://github.com/Lany-Java/StudyNotes/blob/master/SSM/SSM%E6%95%B4%E5%90%88/img/Mybatis%E7%9A%84%E8%BF%90%E8%A1%8C%E6%B5%81%E7%A8%8B.png)

1. 首先SqlSessionFactory会话工厂通过Resources资源信息加载对象获取SqlMapConfig.xml配置文件的信息
2. 然后产生可以与数据库进行交互的会话实例类SqlSession
3. 会话实例类SqlSession可以根据Mapper配置文件中的SQL配置，去执行相应的增删改查操作。
4. 在SqlSession类内部，是通过Executor（分为基本执行器和缓存执行器）对数据库进行操作
5. 执行器Executor与数据库交互，依靠的是底层封装对象Mappered Statement，它封装了从Mapper文件中读取的信息（包括SQL语句、输入参数、输出结果类型）
6. 通过执行器Executor与底层封装对象Mappered Statement的结合，Mybatis就实现了与数据库进行交互的功能


> 参考文章：[https://www.jianshu.com/p/fcb69c6e2bf3](https://www.jianshu.com/p/fcb69c6e2bf3)</br>
> 参考文章：[关于Spring IOC (DI-依赖注入)你需要知道的一切](https://blog.csdn.net/javazejian/article/details/54561302#%E5%9F%BA%E4%BA%8Eautowired%E6%B3%A8%E8%A7%A3%E7%9A%84%E8%87%AA%E5%8A%A8%E8%A3%85%E9%85%8D)</br>
> 参考书目：SpringMVC + Mybatis开发从入门到项目实战 朱要光编著
