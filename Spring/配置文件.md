# 配置文件
 
 **1. <bean>标签，用来创建对象**
 
 ```
 <bean id = "user1" name = "user2" >
 ```
 
 id存在时，id是唯一标识符，name作为别名，当id不存在时，name就作为唯一标识符
 
 **2. <alias>标签，用来设置别名**
 
 ```
 <alias name = "user1" alias= "user2">
 ```
 
 name的值是bean的id或name，alias的值是别名
 
 **3. <import>标签，用来导入其他的xml文件**
 
 ```
 <import resource = "bean.xml">
 ```
 
 resource的值是要导入的xml文件的路径
