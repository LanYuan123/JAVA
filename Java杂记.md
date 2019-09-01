### MVC架构
    MVC是一种架构设计模式，是一种设计理念。
    是为了达到分层设计的目的，从而使代码解耦，便于维护和代码的复用。
    MVC是3个单词的缩写，全称：Model-View-Controller（模型-视图-控制器）。
    
举个例子来说，MVC就好比我们的鞋柜。当没有鞋柜的时候，鞋子是这样摆放的：

![乱](https://github.com/Lany-Java/Java/blob/master/img/%E4%B9%B1%E9%9E%8B%E6%9F%9C.jpg)

有了鞋柜之后，我们是这样摆放的：

![整齐](https://github.com/Lany-Java/Java/blob/master/img/%E5%A5%BD%E9%9E%8B%E6%9F%9C.jpg)

一眼就能看出，有了鞋柜之后，鞋子的摆放明显的整齐和有序很多，这样也很方便我们找到自己想穿的鞋子，不用将大量的时间花在寻找鞋子上。如果把我们的成千上万行代码和各种复杂的业务逻辑看作是各式各样的鞋子，那我们的MVC就是鞋柜。MVC让你的代码结构更加清晰明了

没有使用MVC的时候，我们的代码结构如下：

![](https://github.com/Lany-Java/Java/blob/master/img/20170111110946683.png)

使用MVC分层设计之后，我们的代码结构如下：

![](https://github.com/Lany-Java/Java/blob/master/img/20170111111755043.png)

MVC其实就是提供一种规则，让你把相同类型的代码放在一起，这样就形成了层次，从而达到分层解耦、复用、便于测试和维护的目的。

我们结合我们实际开发中的代码类型来解释一下MVC

1. Model

模型层，可以简单理解就是数据层，用于提供数据。在项目中，（简单理解）一般把数据访问和操作，比如将对象关系映射这样的代码作为Model层，也就是对数据库的操作这一些列的代码作为Model层。比如代码中我们会写DAO和DTO类型的代码，那这个DAO和DTO我们可以理解为是属于Model层的代码。

2. View

视图层，就是UI界面，用于跟用户进行交互。一般所有的JSP、Html等页面就是View层。

3. Controller

控制层，Controller层的功能就是将Model和View层进行关联。比如View主要是显示数据的，但是数据又需要Model去访问，这样的话，View会先告诉Controller，然后Controller再告诉Model，Model请求完数据之后，再告诉View。这样View就可以显示数据了。
