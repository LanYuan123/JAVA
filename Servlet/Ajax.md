# Ajax:一种浏览器端的局部刷新技术（可以异步亦可以同步）
    需求：
        有时候我们需要将本次的响应结果和前面的响应结果内容在同一个页面中展示给用户
    解决：
        1. 在后台服务器端将多次响应内容重新拼接成一个JSP页面，响应
        但是这一样会造成很多响应内容被重复的响应，造成资源的浪费
        2. 使用Ajax技术

    
    
**目录：**
1. [Ajax的使用步骤](#1-ajax的使用步骤)
2. [Ajax的异步和同步](#2-ajax的异步和同步)
3. [Ajax的响应数据格式](#3-ajax响应数据格式)
4. [Ajax的状态值(readyState)](#4-ajax的状态值)
5. [Ajax的请求方式](#5-ajax的请求方式)

---    
## 1. Ajax的使用步骤

```
// 1. 创建Ajax引擎对象
            var ajax;
            if(!(ajax = new XMLHttpRequest())){
                ajax = new ActiveXObject("Msxm12.XMLHTTP");
            }
            // 2. 覆写onreadystatechange函数
            ajax.onreadystatechange = function () {
                // 3. 判断Ajax状态码
                if(ajax.readyState == 4){
                    // 4. 判断响应状态码
                    if(ajax.status == 200){
                        // 5. 获取响应数据
                        var result = ajax.responseText;
                        // 6. 处理响应数据,js操作文档结构
                        alert(result);
                    }
                }
            }
            // 7. 发送请求
            ajax.open("get","ajax");
            ajax.send(null);
```

---

## 2. Ajax的异步和同步

Ajax.open(method,url,async)

async:设置同步代码执行还是异步代码执行

true代表异步，默认是异步

false代表同步

---

## 3. Ajax响应数据格式

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
    
---

## 4. Ajax的状态值
    在Ajax运行过程中，对于访问XMLHttpRequest不是一次就完成的，而是经历多种状态后获取的结果。
    
对于这种状态在Ajax中分为5种状态，有五种状态值：
- 0：(未初始化)XMLHttpRequest已建立，还没有调用send()方法
- 1：(载入)已经调用open()方法，但send()方法并未被调用
- 2：(载入完成)send()已经执行完成，已经接受到全部的响应内容
- 3：(交互)正在解析相应内容
- 4：(完成)响应内容已经解析完成，用户可以调用
   
**在Ajax中状态值由Ajax.readyState获取(0 ~ 4)**


### Ajax的状态码(即响应码)
    ajax状态码是值，ajax无论请求是否成功，根据http所提及的用户信息，用服务器返回http头信息代码，使用ajax.state来获得

响应码列表：
```
100——客户必须继续发出请求
101——客户要求服务器根据请求转换HTTP协议版本
200——交易成功
201——提示知道新文件的URL
202——接受和处理、但处理未完成
203——返回信息不确定或不完整
204——请求收到，但返回信息为空
205——服务器完成了请求，用户代理必须复位当前已经浏览过的文件
206——服务器已经完成了部分用户的GET请求
300——请求的资源可在多处得到
301——删除请求数据
302——在其他地址发现了请求数据
303——建议客户访问其他URL或访问方式
304——客户端已经执行了GET，但文件未变化
305——请求的资源必须从服务器指定的地址得到
306——前一版本HTTP中使用的代码，现行版本中不再使用
307——申明请求的资源临时性删除
400——错误请求，如语法错误
401——请求授权失败
402——保留有效ChargeTo头响应
403——请求不允许
404——没有发现文件、查询或URl
405——用户在Request-Line字段定义的方法不允许
406——根据用户发送的Accept拖，请求资源不可访问
407——类似401，用户必须首先在代理服务器上得到授权
408——客户端没有在用户指定的饿时间内完成请求
409——对当前资源状态，请求不能完成
410——服务器上不再有此资源且无进一步的参考地址
411——服务器拒绝用户定义的Content-Length属性请求
412——一个或多个请求头字段在当前请求中错误
413——请求的资源大于服务器允许的大小
414——请求的资源URL长于服务器允许的长度
415——请求资源不支持请求项目格式
416——请求中包含Range请求头字段，在当前请求资源范围内没有range指示值，请求也不包含If-Range请求头字段
417——服务器不满足请求Expect头字段指定的期望值，如果是代理服务器，可能是下一级服务器不能满足请求
500——服务器产生内部错误
501——服务器不支持请求的函数
502——服务器暂时不可用，有时是为了防止发生系统过载
503——服务器过载或暂停维修
504——关口过载，服务器使用另一个关口或服务来响应用户，等待时间设定值较长
505——服务器不支持或拒绝支请求头中指定的HTTP版本
```
一般常用：

响应码 | 说明
---|:--
200|表示一切都OK
404|资源未找到
500|内部服务器错误
505|服务器不支持或拒绝支请求头中指定的HTTP版本

在使用Ajax时，我们要判断响应内容解析完成同时也要判断服务器返回表示一切正常的200状态码</br>
这就是我们在使用AJAX时为什么采用下面的方式判断所获得的信息是否正确的原因。
```
if(ajax.readyState == 4 && ajax.status == 200) { 
    putData(ajax.responseText);
}
```
---

## 5. Ajax的请求方式
    两种请求方式：get和post
    
### get请求

```
ajax.open("get","url")；
ajax.send(null);
```

- 通过url直接传参
get的***请求实体拼接在URL的后面**，使用?隔开，参数使用键值对的方式

- 注意：get方式提交经常会遇到浏览器缓存问题，浏览器不对同样的url重复提交。这时可以在url后面增加参数：</br>
?rand = Math.random() 或者：rand = new Date()



### post请求

```
ajax.open("post",url);
ajax.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
ajax.send("a=3&b=4");
```

有单独的请求实体,请求参数写在send中，参数依然是键值对的形式

服务器在接收ajax引擎发送来的post请求时，会默认以字符串的形式处理请求实体中的内容，</br>
因为我们发送的时候是键值对，所以需要加上：</br>
```
ajax.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
```
这样服务器才会以键值对来解析ajax的post请求实体

