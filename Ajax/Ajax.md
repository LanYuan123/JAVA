# Ajax:一种局部刷新技术（可以异步亦可以同步）
    可以实现在当前结果页中显示其他请求的响应结果
    
### Ajax的使用

- 创建Ajax引擎对象
- 覆写onreadystatement函数
   - 判断Ajax状态码
      - 判断响应状态码
         - 获取响应内容
         - 处理响应内容(js操作文档结构)
- 发送请求

### Ajax的异步和同步

Ajax.open(method,url,async)

async:设置同步代码执行还是异步代码执行

true代表异步，默认是异步

false代表同步
