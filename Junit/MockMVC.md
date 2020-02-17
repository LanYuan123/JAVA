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
  
  - 通过MockMvcRequestBuilders的post，get,delete等静态方法构建出MockHttpServletRequestBuilder对象，不同的方法代表不同的http请求（比如post方法就代表post方法的http请求），方法中的参数即是请求的URL,设置好请求对象后，通过MockMVC对象的perform方法执行，模拟出一个Http请求
  
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

### 设置json串

  将一个json串作为参数传入：
  ```
  
  ```

### 设置请求的Content-Type

  设置Request的Content-Type：
  ```
  
  ```

### 设置响应的Content-Type

  设置Response的Content-Type：
  ```
  
  ```

### 打印reponse的信息

  将服务器响应的Http所有信息打印出来：
  ```
  
  ```

### 打印响应过来的信息

  将服务器返回的信息(比如字符串)打印出来:
  ```
  
  ```

### 该次测试成功与否判断

  对该次请求的响应状态码进行判断：
  ```
  
  ```

> 参考文章：[翻译:Spring MVC Test Framework--MockMvc使用](https://misakatang.cn/2018/10/18/%E7%BF%BB%E8%AF%91-Spring-MVC-Test-Framework-MockMvc%E4%BD%BF%E7%94%A8/)</br>
>参考视频：[MockMVC学习](https://www.bilibili.com/video/av81751501?p=7)
