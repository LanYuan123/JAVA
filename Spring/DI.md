# 依赖注入(DI)
    依赖：
      指bean对象创建依赖于容器，bean对象的依赖资源
    注入：
      指bean对象依赖的资源由容器来设置和装配
      
----

# Spring的两种注入方式 ---  构造器注入和setter注入


## 构造器注入

见先前的[IOC创建对象的三种方式](https://github.com/Lany-Java/Java/blob/master/Spring/IOC.md)
      
## setter注入
    要求被注入的属性必须有set方法。set方法的方法名由set加上属性首字母大写。
    如果属性是boolean没有get方法，并且是is加上属性名
    
### 1. 常量注入

   字符串、整数、浮点数
   ```
   <bean id="emp" class="bean.emp">
       <property name="age" value="lanyuan"></property>
   </bean>
   ```

### 2. bean注入

   ```
   <bean id ="user" class="bean.User">
       <property name="emp1" ref="emp"></property>
   </bean>
   ```

### 3. 数组注入

```
<bean id ="user" class="bean.User">
    <property name="arry">
        <array>
            <value>1</value>
            <value>2</value>
            <value>3</value>
        </array>
    </property>
</bean>
```

### 4. List注入

```
<bean id ="user" class="bean.User">
    <property name="list">
        <list>
            <value>英雄联盟</value>
            <value>穿越火线</value>
            <value>守望先锋</value>
            <value>魔兽争霸</value>
        </list>
    </property>
</bean>
```

### 5. Map注入

```
<bean id ="user" class="bean.User">
    <property name="cards">
        <map>
            <entry key="中国银行" value="123456123456"></entry>
            <entry>
                <key><value>建设银行</value></key>
                <value>897654987654</value>
            </entry>
        </map>
    </property>
</bean>
```

### 6. Set注入

```
<bean id ="user" class="bean.User">
    <property name="books">
        <set>
            <value>Java核心卷1</value>
            <value>Java核心卷2</value>
            <value>Java编程思想</value>
            <value>Java</value>
        </set>
    </property>
</bean>
```

### 7. NULL注入

```
<bean id ="user" class="bean.User">
    <property name="wife">
        <null></null>
    </property>
</bean>
```

### 8. properties注入

```
<bean id ="user" class="bean.User">
		<property name="info">
				<props>
						<prop key="学号">2018110124</prop>
						<prop key="姓名">ssjjsj</prop>
						<prop key="年龄">19</prop>
				</props>
		</property>
</bean>
```

### 9. P命名空间注入

必须在头文件中加入
```
xmlns:p="http://www.springframework.org/schema/p"
```

beans.xml
```
<!-- p命名空间注入，要求有set方法-->
<bean id ="user" class="vo.User" p:name="lanyuan" p:age="19">
        
</bean>
```

### 10. C命名空间注入

必须在头文件中加入
```
xmlns:p="http://www.springframework.org/schema/c"
```

beans.xml
```
<!-- c命名空间注入，要求有相应参数的构造方法-->
<bean id ="user" class="vo.User" c:name="lanyuan" c:age="16">

</bean>
```
