# 目录

1. [MyBatis初识](#1mybatis初识)
2. [MyBatis的使用](#2mybatis的使用)

---

## 1.Mybatis初识
    MyBatis是一个基于Java的持久层框架
    是一个半自动化的ORM框架
    O--Object，R--Relationship，M--Mapping

持久化：数据从瞬时状态变为持久状态

持久层：完成持久化工作的代码块 --- 即Dao层

简单来说，**MyBatis就是帮助程序员将数据存入数据库中，和从数据库中取数据**

**功能**：
  - 传统的JDBC操作：有很多重复代码块。比如：数据取出时的封装、数据库的建立连接等，
我们通过框架可以减少重复代码，提高开发效率
  - MyBatis是支持普通SQL查询，存储过程和高级映射的优秀持久层框架。MyBatis消除了几乎所有的JDBC代码和参数的手工设置以及结果集的检索
  
---
  
## 2.MyBatis的使用

要使用MyBatis，只需要将mybatis-x.x.x.jar文件以及一些依赖文件置于classpath中即可

如果使用Maven来构建项目，则需要将下面的dependency代码置于pom.xml文件中
```
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>x.x.x</version>
</dependency>
```

使用步骤：

  1. **导入MyBatis相关的jar包**
  
  lib包
  ```
  ant-1.10.3.jar
  ant-launcher-1.10.3.jar
  asm-7.0.jar
  cglib-3.2.10.jar
  commons-logging-1.2.jar
  javassist-3.24.1-GA.jar
  log4j-1.2.17.jar
  log4j-api-2.11.2.jar
  log4j-core-2.11.2.jar
  mybatis-3.5.1.jar
  ognl-3.2.10.jar
  slf4j-api-1.7.26.jar
  slf4j-log4j12-1.7.26.jar
  ```
  
  2. **编写MyBatis核心配置文件**
  
  建立mybatis.config.xml文件
  ```
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
  <configuration>
    <typeAliases>
      <typeAlias alias="User" type="com.yihaomen.mybatis.model.User"/>
    </typeAliases>
    <environments default="development">
      <environment id="development">
        <transactionManager type="JDBC"/>
        <dataSource type="POOLED">
          <property name="driver" value="com.mysql.jdbc.Driver"/>
          <property name="url" value="jdbc:mysql://127.0.0.1:3306/mybatis" />
          <property name="username" value="root"/>
          <property name="password" value="password"/>
        </dataSource>
      </environment>
    </environments>
    <mappers>
      <mapper resource="com/yihaomen/mybatis/model/User.xml"/>
    </mappers>
  </configuration>
  ```
  
  3. **创建SqlSessionFactory，以及获得SqlSession**
  
  创建一个类MyBatisUtil
  ```
  public class MyBatisUtil {
			public static SqlSessionFactory getSqlSessionFactory() throws IOException {
					String resource = "org/mybatis/example/mybatis-config.xml";
					InputStream inputStream = Resources.getResourceAsStream(resource);
					SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
					return sqlSessionFactory;
			}

			public static SqlSession getSession() throws IOException {
					SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
					return sqlSessionFactory.openSession();
			}
	}
  ```
  
  4. **创建实体类**
  
  创建实体类User
  ```
  public class User {
      private int id;
      private String name;
      private String pwd;

      public int getId() {
          return id;
      }

      public void setId(int id) {
          this.id = id;
      }

      public String getName() {
          return name;
      }

      public void setName(String name) {
          this.name = name;
      }

      public String getPwd() {
          return pwd;
      }

      public void setPwd(String pwd) {
          this.pwd = pwd;
      }
  }
  ```
  
  5. **编写sql语句的映射文件**
  
  创建sql语句的映射文件
  ```
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <mapper namespace="entity.UserMapper">

      <select id="selectUser" resultType="User">
      select * from User where id = #{id}
      </select>

  </mapper>
  ```
  
  6. **测试**
  ```
  public class test {
      public static void main(String[] args) throws IOException {
          SqlSession session = MyBatisUtil.getSession();
          User user = session.selectOne("UserMapper.selectUser","1");
          System.out.println(user.getUsername());
          session.close();
      }
  }
  ```
  
