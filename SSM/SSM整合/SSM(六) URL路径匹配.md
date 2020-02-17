# URL路径匹配

## /

会匹配到 /springmvc 这样的路径型url，不会匹配到模式为\*.jsp这样的后缀型url

## /*

会匹配所有的url,路径型的和后缀型的url(包括/springmvc，.jsp，.js和\*.html等)的URL

## /*.action

会匹配相应后缀（例如.action）的URL

# classpath:和classpath*:

可以使用classpath:和classpath*:前缀加路径的方式从classes包下加载文件

- classpath：只能加载找到的第一个文件
- classpath* ：从多个jar文件中加载相同的文件
- 如果要加载的资源，不在当前ClassLoader的路径里，那么用classpath:前缀是找不到的，这种情况下就需要使用classpath*:前缀
- 在多个classpath中存在同名资源，都需要加载时，那么用classpath:只会加载第一个，这种情况下需要用classpath*:前缀

比如 resource1.jar中的package 'com.test.rs' 有一个 'jarAppcontext.xml' 文件,内容如下:
```
<bean name="ProcessorImplA" class="com.test.spring.di.ProcessorImplA" />
```
resource2.jar中的package 'com.test.rs' 也有一个 'jarAppcontext.xml' 文件,内容如下:
```
<bean id="ProcessorImplB" class="com.test.spring.di.ProcessorImplB" />
```
通过使用下面的代码则可以将两个jar包中的文件都加载进来
```
ApplicationContext ctx = new ClassPathXmlApplicationContext( "classpath*:com/test/rs/jarAppcontext.xml");
```
而如果写成下面的代码,就只能找到其中的一个xml文件(顺序取决于jar包的加载顺序)
```
ApplicationContext ctx = new ClassPathXmlApplicationContext( "classpath:com/test/rs/jarAppcontext.xml");
```

</br>
</br>

> 关于这个问题在StackOverflow上有更详细的解释：[https://stackoverflow.com/questions/4140448/difference-between-and-in-servlet-mapping-url-pattern](https://stackoverflow.com/questions/4140448/difference-between-and-in-servlet-mapping-url-pattern)</br>
> 参考文章：[Spring加载resource时classpath*:与classpath:的区别](https://blog.csdn.net/kkdelta/article/details/5507799)
