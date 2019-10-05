## 目录

1. [SpringMVC概述](#springmvc的概述)
2. [SpringMVC的优势](#springmvc的优势)
3. [SpringMVC和Struts2的优劣分析](#springmvc和struts2的优劣分析)
4. [SpringMVC的入门使用流程
](#springmvc的入门使用流程)

## SpringMVC概述

SpringMVC是一种基于Java实现MVC设计模型的请求驱动类型的轻量级**Web框架**

属于Spring Framework系列的后续产品，
已经融合在Spring Web Flow里面。Spring框架提供了构建Web应用程序的全功能MVC模块，使用Spring可插入的MVC架构，
从而在使用Spring进行Web开发时，可以选择使用Spring的SpringMVC框架或集成其他MVC框架，如Structs11(现在一般不用)，Structs2等

Spring已经成为目前最主流的MVC框架之一，并且随着Spring3.0的发布，全面超越Struts2，成为最优秀的MVC框架

它通过一套注解，使一个普通的Java类成为处理请求的控制器，而且无需实现任何接口，同时它还支持Restful编程风格的请求

## SpringMVC的优势

1. 清晰的角色划分：
    - 前端控制器(DispatcherServlet)
    - 请求到处理器映射(HandlerMapping)
    - 处理器适配器(HandlerAdapter)
    - 视图解析器(ViewResolver)
    - 处理器或页面控制器(Controller)
    - 验证器(Validtor)
    - 命令对象(Command 请求参数绑定到的对象就叫命令对象)
    - 表单对象(Form Object 提供给表单展示和提交到的对象就叫表单对象)
2. 分工明确，而且扩展点相当灵活，可以很容易扩展，虽然几乎不需要。
3. 由于命令对象就是一个POJO，无需继承框架特定API，可以使用命令对象直接作为业务对象
4. 和Spring的其他框架无缝集成，是其他Web框架不具备的
5. 可适配，通过HandlerAdapter可以支持任意的类作为处理器
6. 可定制性，HandlerMapping和ViewResolver等能非常简单的定制
7. 功能强大的数据验证、格式化、绑定机制
8. 利用Spring提供的Mock对象能非常简单的进行Web层单元测试
9. 本地化，主题的解析的支持，使我们更容易进行国际化和主题的切换
10. 强大的JSP标签库
11. Restful风格的支持，简单的文件上传，约定大于配置的契约式编程，基于注解的零配置支持

## SpringMVC和Struts2的优劣分析
    共同点：
      1. 都是表现层框架，都是基于MVC模型编写的
      2. 他们的底层都离不开原始ServletAPI
      3. 他们处理请求的机制都是一个核心控制器
      
    区别：
      1. SpringMVC的入口是Servlet，Struts2的入口是Filter
      2. SpringMVC是基于方法设计的，而Struts2是基于类，Struts2每次执行都会创建一个动作类。所以SpringMVC会性能比Struts2好
      3. SpringMVC使用更加简洁，同时还支持JSR303，处理ajax的请求更加方便
      (JSR303是一套JavaBean的参数校验的标准，它定义了很多校验常用的注解，
      我们可以直接将这些注解加在我们的JavaBean的属性上，就可以在需要校验的时候校验了)
      4. Struts2的OGNL表达式使页面的开发效率比SpringMVC更高，但是执行效率没有比JSTL更高，
      尤其是Struts2的表单标签，远没有html执行效率高
      
      
## SpringMVC的入门使用流程



