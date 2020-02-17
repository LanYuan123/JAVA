# HTTP之Content-Type请求头的作用

## Content-Type定义

> MediaType即是Internet Media Type，互联网媒体类型；也叫MIME类型，在Http协议消息头中，使用Content-Type来表示具体请求中的媒体类型信息

常见媒体格式如下：

- text/html：HTML格式
- text/plain：纯文本格式
- text/xml：XML格式
- image/gif：gif图片格式
- image/jpeg：jpg图片格式
- image/png：png图片格式

以application开头的媒体格式类型：

- application/xhtml+xml：XHTML格式
- application/xml： XML数据格式
- application/atom+xml：Atom XML聚合格式
- application/json： JSON数据格式
- application/pdf：pdf格式
- application/msword： Word文档格式
- application/octet-stream： 二进制流数据（如常见的文件下载）
- application/x-www-form-urlencoded：\<form encType=””\>默认的encType，form表单数据被编码为key/value格式发送到服务器（表单默认的提交数据的格式）

另外一种常见的媒体格式是上传文件之时使用的：

- multipart/form-data： 需要在表单中进行文件上传时，就需要使用该格式

## Content-Type实例说明

上面算是基本定义和取值，下面结合实例对典型的几种方式进行说明

- application/x-www-form-urlencoded：数据被编码为名称/值对。这是标准的编码格式
- multipart/form-data： 数据被编码为一条消息，页上的每个控件对应消息中的一个部分
- text/plain： 数据以纯文本形式(text/json/xml/html)进行编码，其中不含任何控件或格式字符

对于前端使用而言，form表单的enctype属性为编码方式，常用有两种：application/x-www-form-urlencoded和multipart/form-data，默认为application/x-www-form-urlencoded。

### Get请求

发起Get请求时，浏览器用application/x-www-form-urlencoded方式，将表单数据转换成一个字符串(key1=value1&key2=value2...)拼接到url上，
这就是我们常见的url带请求参数的情况

### Post请求

发起post请求时，如果没有传文件，浏览器也是将form表单的数据像application/x-www-form-urlencoded方式一样，封装成key=value的结果丢到http body中，如果有传文件的场景，Content-Type类型会升级为multipart/form-data

> 参考文章：[Spring之RequestBody的使用姿势小结](https://juejin.im/post/5b5efff0e51d45198469acea)
