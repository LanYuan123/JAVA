# Ajax响应数据格式

Ajax可以使用三种响应数据格式一种是字符串，一种为Json，一种为XML。

### Json（重点）:responseText
    数据按照Json的格式凭借好的字符串，方便使用eval()方法将接受的字符串数据直接转换为js对象
    
Json格式：
```
var 对象名 = {
      属性名:属性值,
      属性名:属性值,
      属性名:属性值,
      .....
    }
```

因为手动写为Json格式会有些麻烦，所以我们可以使用Gson工具包，传入对象自动转化为Json格式
```
resp.getWriter().write(new Gson().toJson(user));
```

JS:转化Java对象为Json对象
```
var result = ajax.responseText;
eval("var u=" + result);
```

### XML:responseXML，放回document对象
    通过document对象将数据从xml中获取出来
    
