## Ajax的请求方式
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
