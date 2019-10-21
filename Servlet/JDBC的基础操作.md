# JDBC(Java DataBase Connection)
    本质：是官方定义的一套操作所有关系型数据库的规则，即接口。各个数据库厂商去实现这套接口，提供数据库驱动jar包。
    我们可以使用这套接口(JDBC)编程，真正执行的代码是驱动jar包中的实现类
    
步骤：

1. 导入驱动jar包
2. 注册驱动
3. 获取数据库连接对象Connection
4.定义sql
5. 获取执行sql语句的对象Statement
6.执行sql，接受返回结果
7. 处理结果
8. 释放资源

JDBC中主要是5大对象：
 - **DriverManager:  驱动管理对象**
 - **Connection:  数据库连接对象**
 - **Statement:  执行sql对象**
 - **ResultSet:  结果集对象,封装结果集对象**
 - **PreparedStatement:  执行sql对象**
 
 ### :peach:DriverManager
 
 - 注册驱动
    其中有静态方法 ：static void registerDriver(Driver driver)   注册给定的驱动程序
    但是我们写代码的时候通常是使用：
    ```
    Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
    ```
    
    那么二者之间究竟有什么关系呢？
    
    其实在com.microsoft.sqlserver.jdbc.SQLServerDriver这个类被加载进内存的时候，它的静态代码块被执行了，通过执行这个静态代码块，注册了驱动。</br>
    在现在的驱动jar包之中，注册驱动的逻辑是可以被省略的，因为当我们没有注册驱动ed时候，驱动jar包中会有一个文件自动注册驱动
    
    从 JDBC API 4.0 开始，DriverManager.getConnection() 方法得到了增强，可自动加载 JDBC 驱动程序。 因此，使用驱动程序 jar 库时,
    应用程序无需调用 Class.forName 方法来注册或加载驱动程序。
    
 - 数据库连接
 
 方法：
 ```
 static connection getConnection(String url);
 ```
 参数：
 
 url：指定连接的地址</br>
 **语法："jdbc:sqlserver://ip地址（域名）:端口号;databaseName=数据库名称;user=用户名;password=密码;"**
 
 ### :peach:Connection
 
 1. 获取执行SQL的对象
 ```
 Statement creatStatement()
 PrepareStatement perpareStatement(String sql)
 ```
 2. 管理事务
 ```
 开启事务 ： setAutoCommit(boolean autommit) :调用该方法的设置参数为false，即开启事务
 提交事务 ： commit()
 回滚事务 ： rollback()
 ```
 ### :peach:Statement
 
 执行SQL语句
 - boolean execute(String sql):可以执行任意的sql语句
 - int excuteUpdate（String sql）:执行DML（insert，update，delete）语句，DDL（对表和库）语句</br>
 返回值：该返回值为影响的行数，可以通过影响的函数判断DML语句是否执行成功，返回值大于等于0执行成功，小于0失败
 - ResultSet executeQuery(String sql):执行DQL（select）语句
 
 ### :peach:ResultSet
 
 Boolean next()将光标向下移动一行  如果下一行有数据返回true，如果没有数据返回false</br>
 getXxx(参数):获取数据</br>
 Xxx是数据类型</br>
 参数可以是整数类型，也可以是字符串类型</br>
 整数表示的是第几列，从1开始</br>
 字符串是列的名字
 
 ### :peach:PreparedStatement
 
 setString(int，String)：设置占位符中的值
 
 int为第几个问号，String为占位符的值

# JDBC的事务管理

 1. 事务的概念：
    
    一个包含多个步骤的业务操作。如果这个业务操作被事务管理，则这多个步骤要么同时成功，要么同时失败
   
 2. 操作
    
    1. 开启事务
    2. 提交事务
    3. 回滚事务
    
 3. 使用Connection对象来管理事务
    
    - 开启事务：setAutoCommit(boolean autoCommit):调用该方法设置参数为false，即开启事务
    
        在执行sql语句之前开启事务
    
    - 提交事务：commit()
    
        当所有sql都执行完提交事务
    
    - 回滚事务：rollback()
    
        在catch中回滚事务
    
    
# 数据库的连接池

    概念：其实就是一个容器(集合)，存放数据库连接的容器
    
  1. 好处：</br>
    1. 节约资源
    2. 用户访问高效
    
  2. 实现：</br>
    1. 标准接口：DataSource  javax.sql包下的
        1. 方法：
            - C3P0：数据库连接池技术
            - Druid：数据库连接池实现技术，由阿里巴巴提供的，基本上是世界上最好的连接池技术
            
  3. C3P0：数据库的实现技术
        - 步骤：
            1. 导入jar包（两个），c3p0-0.9.5.2.jar 还有 mchange-commons-java-0.2.12.jar，不要忘记导入数据库的驱动jar包
            2. 定义配置文件
                - 名称：c3p0.properties 或者 c3p0-config.xml
                ```
                <c3p0-config>
                    <!--使用默认的配置读取连接池对象-->
                    <default-conjfig>
                        <proporty name="driverClass">com.mysql.jdbc.Driver</proporty>
                        <proporty name="jdbcUrl">jdbc://localhost:3306/db4</proporty>
                        <proporty name="user">root</proporty>
                        <proporty name="password">root</proporty>
                        <!--初始化申请的连接数量-->
                        <proporty name="initialPoolSize">5</proporty>
                        <!--最大的连接数量-->
                        <proporty name="maxPoolSize">10</proporty>
                        <!--超时时间-->
                        <proporty name="checkoutTimeout">3000</proporty>
                    </default-conjfig>

                    <name-config name="otherc3p0">
                        <proporty name="driverClass">com.mysql.jdbc.Driver</proporty>
                        <proporty name="jdbcUrl">jdbc://localhost:3306/db5</proporty>
                        <proporty name="user">root</proporty>
                        <proporty name="password">root</proporty>
                        <!--初始化申请的连接数量-->
                        <proporty name="initialPoolSize">5</proporty>
                        <!--最大的连接数量-->
                        <proporty name="maxPoolSize">8</proporty>
                        <!--超时时间-->
                        <proporty name="checkoutTimeout">1000</proporty>
                    </name-config>
                </c3p0-config>
                ```
                - 路径：直接将文件放在src目录下即可
            3. 创建核心对象 数据库连接池对象 ComboPooledDataSource
            4. 获取连接：connection.getConnection()
            5. 归还连接对象：connnection.close()
             
  4. Druid：数据库连接池技术  
     - 步骤
        1. 导入jar包 druid-1.0.9,jar
        2. 定义与加载配置文件
            - 是properties形式的
            - 可以叫任意名称 ，可以放在任意目录下
            druid.properties
            ```
            driverClassName=com.mysql.jdbc.Driver
            url=jdbc:mysql://127.0.0.1:3306/db3
            username=root
            passsword=root
            initialSize=5
            maxActive=10
            maxWait=3000
            ```
            
            加载配置文件
            ```
            Properties properties = new Properties();
            InputStream is = DruidDemo(本类的类名).class.getClassLoader().getResourceAsStream("druid.properties");
            properties.load(is);
            ```
            
        3. 获取连接池对象
        
            ```
            DataSource datasource = DruidDataSourceFactory.createDataSource(properties);
            ```
            
        4. 获取连接
            
            ```
            Connection connection = datasource.getConnection();
            ```
