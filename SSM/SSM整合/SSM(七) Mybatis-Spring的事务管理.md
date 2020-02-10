# Mybatis-Spring的事务管理

## 两种事务管理方式

### 编程式事务管理

- 通过TransactionTemplate手动管理事务，实际应用中很少使用

### 声明式事务管理

使用XML配置 + annotation声明式事务、

## 在Junit单测中使用事务

在Junit单测中使用@Transaction注解来管理事务，不管单测方法是否有异常抛出，单测方法都会自动回滚

在单测中，@Rollback的默认值为true，事务会自动回滚
```
@Test
@Transactional  //事务会自动回滚
public void courseAddTest(){
    Course course1 = new Course();
    course1.setCourseName("30天速上黑铁");
    course1.setContinueSection(4);
    course1.setCourseRemark("无");
    course1.setPriorSections("S1");
    course1.setWeekHours(8);

    Boolean flag = courseService.courseAdd(course1);
    assert flag;
}
```

如果不需要事务回滚，则需要设置@Rollback的值为false

```
@Test
@Transactional
@Rollback(false) //就算抛出了异常，也不会回滚事务
public void courseAddTest(){
    Course course1 = new Course();
    course1.setCourseName("30天速上黑铁");
    course1.setContinueSection(4);
    course1.setCourseRemark("无");
    course1.setPriorSections("S1");
    course1.setWeekHours(8);

    Boolean flag = courseService.courseAdd(course1);
    assert flag;
}
```
所以我们在使用Junit进行单测的时候，使用Spring提供的声明式事务管理，可以防止执行测试类都修改库中的数据，使得数据库数据被污染


> 参考文章：[Spring编程式和声明式事务实例讲解](https://juejin.im/post/5b010f27518825426539ba38)</br>
> 参考文章：[Spring事务之六（JUnit单测事务回滚）](https://blog.csdn.net/zhuqiuhui/article/details/70307141)
