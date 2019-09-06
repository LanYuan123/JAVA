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
 static connection getConnection(String url,String user,String password);
 ```
 参数：
 
 url：指定连接的地址</br>
 **语法：jdbc:sqlserver://ip地址（域名）:端口号/数据库名称**
 
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
