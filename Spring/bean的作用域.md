# bean的作用域

bean标签有一个scope属性，来设置bean的作用域
```
<bean scope = "singleton">
```

bean的作用域

属性|作用
|:--:|:--:
singleton|单例，整个容器中只有一个对象实例，默认是单例
prototype|原型，每次获取bean都产生一个新的对象
request|每次请求时创建一个新的对象
session|在会话的范围内是一个对象
global session|只在portlet下有用，表示是application
application|在应用范围中一个对象
