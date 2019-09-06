# Ajax:一种浏览器端的局部刷新技术（可以异步亦可以同步）
    可以实现在当前结果页中显示其他请求的响应结果
    
### Ajax的使用步骤

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

### Ajax的异步和同步

Ajax.open(method,url,async)

async:设置同步代码执行还是异步代码执行

true代表异步，默认是异步

false代表同步
