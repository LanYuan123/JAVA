## CURD操作
  
  UserDao.java
  ```
  public class UserDao {
      public User getById(int id) throws IOException {
          SqlSession sqlSession = MyBatisUtil.getSession();
          User user = sqlSession.selectOne("entity.UserMapper.selectUser",id);
          sqlSession.close();
          return user;
      }

      public int add(User user) throws IOException {
          SqlSession sqlSession = MyBatisUtil.getSession();
          int result = sqlSession.insert("entity.UserMapper.insertUser",user);
          sqlSession.commit();
          sqlSession.close();
          return result;
      }

      public int update(User user) throws IOException {
          SqlSession sqlSession = MyBatisUtil.getSession();
          int result = sqlSession.update("entity.UserMapper.updateUser",user);
          sqlSession.commit();
          sqlSession.close();
          return result;
      }

      public int delete(int id) throws IOException {
          SqlSession sqlSession = MyBatisUtil.getSession();
          int result = sqlSession.update("entity.UserMapper.deleteUser",id);
          sqlSession.commit();
          sqlSession.close();
          return result;
      }
  }
  ```
  
  注意:</br>
  除了查询，添加，删除，修改操作都被视为事务，所以需要加入sqlSession.commit();提交事务，才会起作用
  
  user.mapper.xml
  ```
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <mapper namespace="entity.UserMapper">

      <select id="selectUser" resultType="entity.User">
          select * from users where uid = #{id}
      </select>

      <insert id="insertUser" parameterType="entity.User">
          insert into users(username,password,uid) values(#{username},#{password},#{uid})
      </insert>

      <update id="updateUser" parameterType="entity.User">
          update users set name=#{username} pwd = #{password} where id=#{uid}
      </update>

      <delete id="deleteUser" parameterType="entity.User">
          delete form users where id=#{id}
      </delete>

  </mapper>
  ```
