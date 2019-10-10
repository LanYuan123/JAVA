## 乱码

乱码的解决---通过过滤器来解决乱码，SpringMVC提供CharacterEncoding来解决post乱码
```
<!--配置解决乱码的过滤器-->
<filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>utf-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

如果是get方式乱码
    1. 修改tomacat的配置解决
    2. 自定义乱码解决的过滤器
    
## restful风格的url

优点：轻量级，安全
```
@Controller
public class TestController {
    @RequestMapping("/{uid}/{id}/test")
    public String test(@PathVariable int uid, @PathVariable int id, ModelMap modelMap){
        System.out.println("this is test!");
        modelMap.addAttribute("msg",uid);
        System.out.println(uid);
        System.out.println(id);
        return "/index.jsp";
    }
}
```

## 同一个controller通过参数来到达不同的处理方法-----不重要

url
```
http://localhost:8080/SpringMVC/test?method=add
```

源码
```
@Controller
@RequestMapping("/test")
public class TestController {
    @RequestMapping(params = "method=add",method = RequestMethod.GET)
    public void add(){
        System.out.println("add");
    }
    @RequestMapping(params = "method=delete",method = RequestMethod.POST)
    public void delete(){
        System.out.println("delete");
    }
}
```

如果设置了method参数，那么只能按照参数给出的方法去接收
