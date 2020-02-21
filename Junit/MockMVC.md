**目录：**
- [MockMVC简介及使用](#mockmvc简介及使用)
  * [MockMVC简介——SpringMVC单元测试的独立测试](#mockmvc简介springmvc单元测试的独立测试)
  * [MockMVC的使用](#mockmvc的使用)
    + [1.添加依赖](#1添加依赖)
    + [实例化MockMVC](#实例化mockmvc)
    + [正式测试](#正式测试)
  * [MockMVC常用功能API](#mockmvc常用功能api)
    + [设置参数](设置参数)
    + [设置json串](设置json串)
    + [设置请求的Content-Type](#设置请求的content-type)
    + [设置响应的Content-Type](#设置响应的content-type)
    + [打印reponse的信息](#打印reponse的信息)
    + [打印响应过来的信息](#打印响应过来的信息)
    + [该次测试成功与否判断](#该次测试成功与否判断)
    + [设置编码格式](#设置编码格式)
    + [向Mock测试中添加Filter](#向mock测试中添加filter)


# MockMVC简介及使用

## MockMVC简介——SpringMVC单元测试的独立测试

对模块进行集成测试时，希望能够通过输入URL对Controller进行测试，如果通过启动服务器，建立http client进行测试，这样会使得测试变得很麻烦，比如，启动速度慢，
测试实验不方便，依赖网络环境等，所以为了可以对Controller进行测试，我们引入了MockMVC

MockMVC实现了对Http请求的模拟，能够直接使用网络的形式，转换到Controller的调用，这样可以使得测试速度快、不依赖网络环境，而且提供了一套验证工具，这样可以
使得请求的验证统一而且很方便

## MockMVC的使用

### 1.添加依赖

  要使用MockMVC，首先我们需要添加两个依赖junit和spring-test
  
  ```
  <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
  </dependency>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>5.1.5.RELEASE</version>
    <scope>test</scope>
  </dependency>
  ```

### 实例化MockMVC

  使用MockMVC对controller层进行单测时，一切都要依赖于MockMVC对象，所以我们首先要实例化MockMVC对象，现在有两种方法实例化MockMVC对象：

  - 第一种：通过TestContext框架加载SpringMVC配置文件，这样会加载Spring的配置信息并且通过注入WebApplicationContext到测试中来构建一个MockMVC实例
    
    ```
    @RunWith(SpringJUnit4ClassRunner.class)
    @ContextConfiguration("classpath:config/applicationContext.xml")
    @WebAppConfiguration
    public class MyWebTests {

        @Autowired
        private WebApplicationContext wac;

        private MockMvc mockMvc;

        @Before
        public void setup() {
            this.mockMvc = MockMvcBuilders.webAppContextSetup(this.wac).build();
     }

     // ...

    }
    ```
    
  - 第二种：手动创建一个controller实例而不加载Spring的配置信息.相对的,通过大致比较MVC JavaConfig或者MVC namespace来自动的创建一个默认的配置.你可以在一定程度上自定义
    
    ```
    @RunWith(SpringJUnit4ClassRunner.class)
    @ContextConfiguration("classpath:config/applicationContext.xml")
    @WebAppConfiguration
    public class MyWebTests {

        private MockMvc mockMvc;

        @Before
        public void setup() {
            this.mockMvc = MockMvcBuilders.standaloneSetup(new AccountController()).build();
        }

        // ...

    }
    ```
    

### 正式测试

  在实例化完MockMVC对象后，我们就可以对controller层进行正式的Mock接口测试了

  MockMVC的使用是基于"流式理念"来进行的
  
  - 通过MockMvcRequestBuilders的post，get,delete,put等静态方法构建出MockHttpServletRequestBuilder对象，不同的方法代表不同的http请求（比如post方法就代表post方法的http请求），方法中的参数即是请求的URL,设置好请求对象后，通过MockMVC对象的perform方法执行，模拟出一个Http请求
  
  ```
  @RunWith(SpringJUnit4ClassRunner.class)
  @ContextConfiguration("classpath:config/applicationContext.xml")
  @WebAppConfiguration
  public class UserControllerTest {

      @Autowired
      private WebApplicationContext applicationContext;

      private MockMvc mockMvc;

      @Before
      public void before(){
          mockMvc = MockMvcBuilders.webAppContextSetup(applicationContext).build();
      }
      
      @Test
      public void loginTest() throws Exception {
          MockHttpServletRequestBuilder builders = post("/user/login");
          mockMvc.perform(builders);
      }
  }    
  ```

## MockMVC常用功能API

MockMvcRequestBuilders的主要API：

方法名|作用
:--:|:--:|
get|模拟get请求
post|模拟post请求
delete|模拟delete请求
put|模拟put请求
fileUpload|文件上传方式的请求
asyncDispatch|创建一个从启动异步处理的请求的MvcResult进行异步分派的RequestBuilder

MockHttpServletRequestBuilder和MockMultipartHttpServletRequestBuilder的常用API：

方法名|作用
:--:|:--:|
param/params|添加请求参数
header/headers|添加头信息
contentType|指定请求的contentType头信息
accept|指定请求的Accept头信息
content|指定请求Body体内容
cookie|指定请求的cookie
locale|指定请求的Locale
characterEncoding|指定请求字符编码
requestAttr|设置请求属性数据
sessionAttr/sessionAttrs|设置请求session属性数据
flashAttr|指定请求的flash信息，比如重定向后的属性信息
session|指定请求的Session
principal|指定请求的Principal
contextPath|指定请求的上下文路径，必须以“/”开头，且不能以“/”结尾
pathInfo|请求的路径信息，必须以“/”开头
secure|请求是否使用安全通道
with|请求的后处理器，用于自定义一些请求处理的扩展点

### 模拟各种方法的请求

  可以通过MockMvcRequestBuilders的多个静态方法（比如get，post，delete，put等）来模拟各种方法的HTTP请求，我们只需要在参数内填入请求路径即可
  
  ```
  @Test
  public void logoutTest() throws Exception {
      MockHttpServletRequestBuilder builders = MockMvcRequestBuilders.post("/user/login");
  }
  ```
  
  该方法最后会返回一个MockHttpServletRequestBuilder对象，将此对象放入到MockMVC对象的perform方法中执行
  
### 提供文件上传方式的请求

  提供文件上传请求的模拟
  
  ```
  byte[] bytes = new byte[] {1, 2};  
  mockMvc.perform(fileUpload("/user/{id}/icon", 1L).file("icon", bytes)) //执行文件上传  
          .andExpect(model().attribute("icon", bytes)) //验证属性相等性  
          .andExpect(view().name("success")); //验证视图  
  5.JSON请求/响应验证
  String requestBody = "{\"id\":1, \"name\":\"zhang\"}";  
      mockMvc.perform(post("/user")  
              .contentType(MediaType.APPLICATION_JSON).content(requestBody)  
              .accept(MediaType.APPLICATION_JSON)) //执行请求  
              .andExpect(content().contentType(MediaType.APPLICATION_JSON)) //验证响应contentType  
              .andExpect(jsonPath("$.id").value(1)); //使用Json path验证JSON 请参考http://goessner.net/articles/JsonPath/   
      String errorBody = "{id:1, name:zhang}";  
      MvcResult result = mockMvc.perform(post("/user")  
              .contentType(MediaType.APPLICATION_JSON).content(errorBody)  
              .accept(MediaType.APPLICATION_JSON)) //执行请求  
              .andExpect(status().isBadRequest()) //400错误请求  
              .andReturn();   
      Assert.assertTrue(HttpMessageNotReadableException.class.isAssignableFrom(result.getResolvedException().getClass()));//错误的请求内容体
  ```
  
  该方法最后会返回一个MockMultipartHttpServletRequestBuilder对象

### 设置参数

  使用param方法进行请求参数的设置：
  ```
  @Test
  public void loginTest() throws Exception {
      MockHttpServletRequestBuilder builders = post("/user/login").
              param("jobNumber","20200217").
              param("passWord","123456");
      mockMvc.perform(builders);
  }
  ```
  
  - 注入对象
  
  ```
  @Test
  public void loginTest() throws Exception {
      MockHttpServletRequestBuilder builders = post("/user/login").
              param("jobNumber","2018110124").
              param("passWord","123456").
              param("sex","1");
      mockMvc.perform(builders);
  }
  ```
  
  - 注入对象中还有联结对象
  
  ```
  @Test
  public void loginTest() throws Exception {
      MockHttpServletRequestBuilder builders = post("/user/login").
              param("jobNumber","2018110124").
              param("passWord","123456").
              param("sex","1").
              param("address.provinceName","广东").
              param("address.cityName","广州");
      mockMvc.perform(builders);
  }
  ```
  
  - 注入对象有List集合
  
  ```
  @Test
  public void loginTest() throws Exception {
      MockHttpServletRequestBuilder builders = post("/user/login").
              param("List[0]","2018110124").
              param("passWord","123456").
              param("sex","1").
              param("list[0].provinceName","广东").
              param("list[1].cityName","广州").
              param("list[0].provinceName","四川").
              param("list[1].cityName","成都");
      mockMvc.perform(builders);
  }
  ```
  
  - 注入对象有Map集合
  
  ```
  @Test
  public void loginTest() throws Exception {
      MockHttpServletRequestBuilder builders = post("/user/login").
              param("List[0]","2018110124").
              param("passWord","123456").
              param("sex","1").
              param("map['key1'].provinceName","广东").
              param("map['key1'].cityName","广州").
              param("map['key2'].provinceName","四川").
              param("map['key2'].cityName","成都");
      mockMvc.perform(builders);
  }
  ```

 - 注入Date对象
 
 ```
 @Test
 public void loginTest() throws Exception {
     MockHttpServletRequestBuilder builders = post("/user/login").
             param("date","2020-02-18");
     mockMvc.perform(builders);
 }
 ```
 
  
### 设置json串

  将一个json串作为参数传入：
  ```
  @Test
  public void loginTest() throws Exception {
      String json = "{\n" +
              "\t\"id\": 20,\n" +
              "\t\"jobNmuber\": \"220200218\",\n" +
              "\t\"passWord\": \"12346\"\n" +
              "}";
      MockHttpServletRequestBuilder builders = post("/user/login").
              accept(MediaType.APPLICATION_JSON).
              content(json);
      mockMvc.perform(builders);
  }
  ```

### 设置请求的Content-Type

  设置Request的Content-Type：
  ```
  @Test
  public void loginTest() throws Exception {
      MockHttpServletRequestBuilder builders = post("/user/login").
              contentType(MediaType.APPLICATION_JSON);
      mockMvc.perform(builders);
  }
  ```

### 设置响应的Content-Type

  设置Response的Content-Type：
  ```
  @Test
  public void loginTest() throws Exception {
      MockHttpServletRequestBuilder builders = post("/user/login").
              accept(MediaType.APPLICATION_JSON);
      mockMvc.perform(builders);
  }
  ```

### 打印reponse的信息

  将服务器响应的Http所有信息打印出来：
  ```
  @Test
  public void loginTest() throws Exception {
      MockHttpServletRequestBuilder builders = post("/user/login").
              accept(MediaType.APPLICATION_JSON).
              param("jobNumber","20200218").
              param("passWord","123456");
      mockMvc.perform(builders).andDo(print());
  }
  ```
  print()是MockMvcResultHandlers类下的静态方法，需要我们静态导入

### 打印响应过来的信息

  将服务器返回的信息(比如字符串)打印出来:
  ```
  @Test
  public void loginTest() throws Exception {
      MockHttpServletRequestBuilder builders = post("/user/login").
              accept(MediaType.APPLICATION_JSON).
              param("jobNumber","20200218").
              param("passWord","123456");
      String result = mockMvc.perform(builders).
              andReturn().getResponse().
              getContentAsString();
      System.out.println(result);
  }
  ```

### 该次测试成功与否判断

  对该次请求的响应状态码进行判断：
  ```
  @Test
  public void loginTest() throws Exception {
      MockHttpServletRequestBuilder builders = post("/user/login").
              accept(MediaType.APPLICATION_JSON).
              param("jobNumber","20200218").
              param("passWord","123456");
      mockMvc.perform(builders).andExpect(status().isOk());
  }
  ```
  status()方法需要静态导入，isOK()方法判断返回状态码是否是200 OK
  
### 设置编码格式

 设置响应的编码格式
 ```
 @Test
 public void loginTest() throws Exception {
     MockHttpServletRequestBuilder builders = post("/user/login").
             characterEncoding("UTF-8");
     mockMvc.perform(builders).andExpect(status().isOk());
 }
 ```
 
 
### 向Mock测试中添加Filter

 在对Controller层使用Mock测试的时候，默认是不会将Filter添加到测试当中的，这需要我们手动去添加

 向Mock测试中添加Filter
 ```
 private MockMvc mockMvc;

 @Before
 public void before(){
     mockMvc = MockMvcBuilders.webAppContextSetup(applicationContext).
             addFilter(new CharacterEncodingFilter()).
             build();
 }
 ```
 在创建MockMvc对象的时候，就需要加上addFilter方法，在这之后的所有用到这个MockMvc对象的地方，都会添加上这个过滤器
 
 
 
### 向请求头中添加信息

 在一些实际开发中，很多情况下我们需要涉及到用户权限问题，就是拿到用户客户端登录后的token，在代码中进行校验，一般都是在controller层首先进行校验，如果校验成功，则执行之后操作，否则采取相应操作，或者返回到登录界面或者错误界面
 
 但是我们进行单测的时候，怎么拿到用户登录时候的token呢，这里就是用模拟登陆实现，也就是我们向模拟的请求头中手动添加token
 
 ```
 @Test
 public void logoutTest() throws Exception {
     MockHttpServletRequestBuilder builders = post("/user/login").
             header("token","token字符串");
 }
 ```
 使用header方法来向请求头中添加数据

 

> 参考文章：[翻译:Spring MVC Test Framework--MockMvc使用](https://misakatang.cn/2018/10/18/%E7%BF%BB%E8%AF%91-Spring-MVC-Test-Framework-MockMvc%E4%BD%BF%E7%94%A8/)</br>
> 参考视频：[MockMVC学习](https://www.bilibili.com/video/av81751501?p=7)</br>
> 参考文章：[使用MockMvc测试 Controller](https://blog.csdn.net/Adam_allen/article/details/79919921)
