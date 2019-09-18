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

---

3. **基本标签**

标签|作用
|:--:|:--:
<c:out value="数据" default="默认值"></c:out>|将数据输出给客户端
<c:set var="hello" value="hello pageContext" scope="page"></c:set>|储存数据到作用域中
<c:remove var="hello" scope="page"/>|删除作用域中的指定键的数据

- ```<c:out value="数据" default="默认值"></c:out>```
    
    value可以为常量值也可以为EL表达式，default是当value为空时默认输出的值

- ```<c:set var="hello" value="hello pageContext" scope="page"></c:set>```

    var：表示存储的键名</br>
    value：表示存储的数据</br>
    scope：表示要存储的作用域对象 page request session application</br>

- ```<c:remove var="hello" scope="page"/>```

    var:表示要删除的键的名字</br>
    scope：表示要删除的作用域(可选)</br>
    注意：如果在不指定作用域的情况下使用改标签删除数据，则会四个作用域对象中的符合要求的数据全部删除

---

4. **逻辑标签**

标签|作用
|:--:|:--:
<c:if test="${表达式}">前端代码</c:if>|进行逻辑判断，相当于java代码的单分支判断
<c:choose> <c:when test="">执行内容</c:when> .... <c:otherwise>执行内容</c:otherwise> </c:choose>|进行多条件的逻辑判断，类似于java中的多分支语句

- ```<c:if test="${表达式}">前端代码</c:if>```

    逻辑判断标签需要一拉EL的运算，也就是表达式中涉及到的数据必须从作用域中获取

- ```<c:choose> <c:when test="">执行内容</c:when>.... <c:otherwise>执行内容</c:otherwise> </c:choose>```

    条件成立只会执行一次，如果都不成立则执行otherwise

---

5. **循环标签**
  
标签|作用
|:--:|:--:
<c:forEach begin="1" end="4" step="2" varStatus="vs">循环体</c:forEach>|循环内容进行处理(常量循环)
<c:forEach item="${lsit}" var="str">循环体</c:forEach>|循环内容进行处理(动态循环)

- ```<c:forEach begin="1" end="4" step="2">循环体</c:forEach>```

    begin：声明循环开始位置</br>
    end：声明循环结束位置</br>
    step：设置步长</br>
    varStatus：声明变量记录每次循环的数据(角标，次数，是否是第一次循环，是否是最后一次循环)</br>
    &nbsp;&nbsp;&nbsp;&nbsp;注意：数据存储在作用域中，需要使用EL表达式获取</br>
    &nbsp;&nbsp;&nbsp;&nbsp;例如：${vs.index} ${vs.count} ${vs.first} ${vs.last}</br>

- ```<c:forEach item="${lsit}" var="str">循环体</c:forEach>```

    item：为声明要遍历的对象，结合EL表达式获取对象</br>
    var：声明变量记录每次循环的结果(即item每个角标存的数据)，需要使用EL表达式来获取
