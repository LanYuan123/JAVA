# SSM框架整合的理解

## Spring

Spring就像是整个项目中装配bean的大工厂，在配置文件中配置好参数，通过调用类的构造方法来实例化对象。</br>
Spring的核心思想是IOC（控制反转），这使得程序员不用在显式的new一个对象，而是让Spring框架帮你来完成这一切，我们只需要从Spring中获取该对象即可

### Spring之IOC

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
1. 前端控制器（DispatcherServlet）
2. 处理器映射器（HandlerMapping）
3. 处理器适配器（HandlerAdapter）
4. 处理器（Hander）
5. 视图解析器（View Resolver）
6. 视图（View）

![SpringMVC处理请求流程](https://github.com/Lany-Java/StudyNotes/blob/master/SSM/SSM%E6%95%B4%E5%90%88/img/SpringMVC%E8%AF%B7%E6%B1%82%E6%B5%81%E7%A8%8B.png)


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


参考文章链接：[https://www.jianshu.com/p/fcb69c6e2bf3](https://www.jianshu.com/p/fcb69c6e2bf3)</br>
参考书目：SpringMVC + Mybatis开发从入门到项目实战 朱要光编著
