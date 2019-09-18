# JSTL标签
    JSTL是apache对EL表达式的扩展(也就是说JSTL依赖EL)、JSTL是标签语言！JSTL标签使用依赖非常方便，
    只不过他不是JSP内置的标签，需要我们自己导报，以及指定标签库而已！
    
作用：提高JSP中逻辑代码的编写效率

使用：
1. JSTL的核心标签库(重点)
2. JSTL的格式化标签库(了解)
3. JSTL的SQL标签库(了解)
4. JSTL的函数标签库(了解)
5. JSTL的XML标签库(了解)

## JSTL的核心标签库

1. 导入jar包
2. 声明JSTL标签库的引入(核心标签库)
```
<%@taglib prefix = "c" uri = "http://java.sun.com/jsp/jstl/core" %>
```

3. **基本标签**

标签|作用
|:--:|:--:
<c:out value="数据" default="默认值"></c:out>|将数据输出给客户端
<c:set var="hello" value="hello pageContext" scope="page"></c:set>|储存数据到作用域中
<c:remove var="hello" scope="page"/>|删除作用域中的指定键的数据


4. **逻辑标签**

标签|作用
|:--:|:--:
<c:if test="${表达式}">前端代码</c:if>|进行逻辑判断，相当于java代码的单分支判断
<c:choose> <c:when test="">执行内容</c:when>.... </c:choose>|进行多条件的逻辑判断，类似于java中的多分支语句

5. **循环标签**
  
标签|作用
|:--:|:--:
<c:forEach begin="1" end="4" step="2">循环体</c:forEach>|循环内容进行处理(常量循环)
<c:forEach item="${lsit}" var="str">循环体</c:forEach>|循环内容进行处理(动态循环)
