# 目录

1. [目前开发中存在的问题](#目前开发中存在的问题)
2. [Maven简介](#maven简介)
3. [Maven的安装](#maven的安装)
4. [Maven的核心概念](#maven的核心概念)

## 目前开发中存在的问题

目前的技术在开发中存在的问题

1. 一个项目就是一个工程

   - 如果项目非常庞大，就不适合继续使用package来划分模块，最好是每一个模块都对应一个工程，利于分工协作
   - 借助于Maven就可以将一个项目拆分成多个工程
   
2. 项目中需要的jar包必须手动"复制"、"粘贴"到WEB-INF/lib目录下

   - 带来的问题是：同样的jar包重复出现在不同的项目工程中，一方面浪费存储空间，另外也会让工程显得很臃肿
   - 借助于Maven，可以将jar包仅仅保存在"仓库"中，有需要使用的工程"引用"这个文件接口，并不需要真的包jar包复制过来
   
3. jar需要别人替我们准备好，或者我们到官网下载

   - 不同技术的官网提供jar包下载的形式是五花八门的
   - 有些技术的官网就是通过Maven或SVN等专门的工具来下载的
   - 如果是以不规范的方式下载的jar包，那么其中的内容很可能也是不规范的
   - 借助于Maven，我们可以以一种规范的方式下载jar包，因为所有知名的框架或者第三方工具的jar包以及按照统一的规范存放在了Maven的中央仓库中，以规范方式下载的jar包，内容也是可靠的
   - Tips："统一的规范"，不仅是对IT开发领域非常的重要，对于整个人类社会都是非常重要的。
   
4. 一个jar包依赖的其他jar包需要自己手动添加到项目中
   
   - 如果所有的jar包之间的依赖关系都需要程序员自己非常清楚的了解，那么就会极大的增大学习成本
   - 借助于Maven，它会自动将被依赖的jar包导入进来
   
   
## Maven简介
    
1. Maven是一款服务于Java平台的自动化构建工具
    Make-->Ant-->Maven-->Gradle
2. 构建

      概念：以"Java源文件"、"框架配置文件"、"JSP"、"HTML"、"图片"等资源为原材料，去生产一个可运行的项目的过程
      - 编译：**Java源文件[User.Java]-->编译-->Class字节码文件[User.class]-->交给JVM去执行**
      - 部署：**一个BS项目最终运行的并不是动态Web工程本身，而是这个动态Web工程"编译的结果"**
      
      动态Web工程-->编译、部署-->编译结果
      
      ![web工程](https://github.com/Lany-Java/Java/blob/master/img/Web%E5%B7%A5%E7%A8%8B%E7%BC%96%E8%AF%91.png)
      
      开发过程中，所有路径或者配置文件中配置的类路径等都是以编译结果的目录结构为标准的。
      
      运行时环境
      
      ![RT](https://github.com/Lany-Java/Java/blob/master/img/rt.png)
      
 3. 构建过程中的各个环节
    1. 清理：将以前编译得到的旧的class字节码文件删除，为下一次编译做准备
    2. 编译：将Java源程序编译成class字节码文件
    3. 测试：自动测试，自动调用Junit程序
    4. 报告：测试程序执行的结果
    5. 打包：动态Web工程打war包，Java工程打jar包
    6. 安装：Maven特定的概念-----将打包得到的文件复制到"仓库"中的指定位置
    7. 部署：将动态Web工程生成的war包复制到Servlet容器的指定目录下，使其可以运行
    
## Maven的安装

1. 检查JAVA_HOME环境变量
2. 解压Maven核心程序的压缩包，放在一个非中文无空格路径下
3. 配置Maven的环境变量
4. 运行mav -v命令去查看Maven版本
    
         
## Maven的核心概念

1. **约定的目录结构**

   - 创建约定的目录结构
       1. 根目录：工程名
       2. src目录：源码
       3. pom.xml：Maven工程的核心配置文件
       4. main目录：存放主程序
       5. test目录：存放测试程序
       6. java目录：存放Java源文件
       7. resource目录：存放框架或其他工具的配置文件
       
   - 为什么要遵守约定的目录结构？
       - Maven要负责我们这个项目的自动化构建，以编译为例，Maven要想自动进行编译，那么他必须知道Java源文件保存在哪里
       - 如果我们自己自定义的东西想要让框架或工具知道，有两种办法
            - 以配置的方式明确告诉框架
            - 遵守框架内部已经存在的约定
       - 约定>配置>编码
       
    - 常用的Maven命令
        - 注意：执行与侯建过程相关的Maven命令，必须进入pom.xml所在的目录
            与构建过程相关：编译、测试、打包、....
        - 常用命令：
            1. mvn clean：清理
            2. mvn compile：编译主程序
            3. mvn test-compile：编译测试程序
            4. mvn test：执行测试
            5. mvn package：打包
            
     - 关于联网问题
        1. Maven的核心程序中仅仅定义了抽象的生命周期，但是具体的工作必须由特定的插件来完成，而插件本身并不包含在Maven的核心程序中
        2. 当我们执行的Maven命令需要用到某些插件时，Maven核心程序会首先到本地仓库中查找
        3. 本地仓库的默认位置：[系统中当前用户的家目录]
        4. Maven核心程序如果在本地仓库中找不到需要的插件，那么它会自动连接外网，到中央仓库下载
        5. 如果无法连接外网，则构建失败
        6. 修改默认本地仓库的位置可以让Maven核心程序到我们事先准备好的目录路下查找插件
            - 找到Maven解压目录的\conf\setting.xml
            - 在setting.xml文件中找到localRepository标签
            - 将<localRepository>/path/to/local/repo</localRepository>从注释中取出来
            - 将标签体内容修改为已经准备好的Maven仓库目录

2. **POM**
    1. 含义：Project Objecj Model 项目对象模型</br>
        DOM Document Object Model 文档对象模型
    2. pom.xml对于Maven工程是核心配置文件，与构建过程相关的一切设置都在这个文件中进行配置</br>
        重要程度相当于web.xml对于动态Web工程
        
3. **坐标**

    1. 数学中的坐标：
        - 在平面上，使用X、Y两个向量可以唯一的定位平面中的任何一个点
        - 在空间中，使用X、Y、Z三个向量可以唯一的定位空间中的任何一个点
    2. Maven的坐标
        使用下面三个向量在仓库中唯一定位一个Maven工程
            1. groupid：公司或组织名倒序+项目名
            例如
            ```
            <groupid>com.atguidu.maven</groupid> 
            ```
            2. artifactid：模块名
            ```
            <artifactid>helo</artifactid>
            ```
            3. version：版本
            ```
            <version>1.0.0</version>
            ```
    3. Maven工程仓库中坐标和路径的对应关系
    ```
    <groupid>org.springframework</groupid> 
    <artifactid>spring-core</artifactid>
    <version>4.0.0.RELEASE</version>
    ```
    
    ```
    org/springframework/spring-core/4.0.0.RELEASE/spring-core-4.0.0.RELEASE.jar
    ```
            

4. **依赖**

    - **Maven解析依赖信息时会到本地仓库中查找依赖的jar包**
         
         对我们自己开发的Maven工程，使用mvn install命令安装后就可以进入仓库
         
    - **依赖的范围**
    
         ![依赖范围](https://github.com/Lany-Java/Java/blob/master/img/%E4%BE%9D%E8%B5%96%E8%8C%83%E5%9B%B4.png)
         
         1. compile
             - 对主程序是否有效：有效
             - 对测试程序是否有效：有效
             - 是否参与打包：参与
         2. test
             - 对主程序是否有效：无效
             - 对测试程序是否有效：有效
             - 是否参与打包：不参与
         3. provided
             - 对主程序是否有效：有效
             - 对测试程序是否有效：有效
             - 是否参与打包：不参与
             - 是否参与部署：不参与
             - 典型例子：servlet-api.jar
             
         ![compile](https://github.com/Lany-Java/Java/blob/master/img/%E4%BE%9D%E8%B5%96compile3.png)
         ![provided](https://github.com/Lany-Java/Java/blob/master/img/%E4%BE%9D%E8%B5%96provided.png)
         
     - **依赖的传递**
     
         依赖之间是有传递性的
         
         1. 好处：可以传递的依赖不必在每个模块工程中都重复声明，在"最下面"的工程中依赖一次即可
         2. 注意：非compile范围的依赖不能传递，所以在各个工程模块中，如果有需要就得重复声明依赖
     
     - **依赖的排除**
     
         有时候我们有一些jar包不希望加入到当前工程，这时候我们就需要把一些依赖排除掉
         
         排除依赖的设置方式：
         
         ```
         <exclusions>
             <exclusion>
                 <groupId>commons-logging</groupId>
                 <artifactId>commons-logging-api</artifactId>
             </exclusion>
         </exclusions>
         ```
     
     - **依赖的原则**
     
         作用：解决工程之间jar包的冲突问题
         
         情景设定1：验证路径最短者优先
         
         情景设定2：验证路径相同时，先声明者优先(先声明指的是dependency标签的声明顺序)
         
     
     - **依赖版本号的统一管理**
     
       假如jar包的版本都是4.0.0，这时我们想同时统一升级为4.1.1，怎么办？手动逐一修改，容易出错，且不可靠
     
       建议配置方式
     
          1. 先使用properties标签内使用自定义标签统一声明版本号
          
          ```
          <properties>
              <spring-version>4.0.0</spring-version>
          </properties>
          ```
          
          2. 然后在需要统一声明版本的位置，使用${自定义标签名}引用声明的版本号
          
          ```
          <version>${spring-version}</version>
          ```
     

5. **仓库**

    - 仓库的分类
        1. 本地仓库：当前电脑上部署的仓库目录，为当前电脑上所有Maven工程服务
        2. 远程仓库
            1. 私服：架设在当前局域网环境下，为当前局域网范围内的所有Maven工程服务
            2. 中央仓库：架设在Internet上，为全世界所有Maven工程服务
            3. 中央仓库镜像：为了分担中央仓库的流量，提升用户访问速度
    - 仓库中保存的内容：Maven工程
            1. Maven自身所需要的插件
            2. 第三方框架或工具的jar包
            3. 我们自己开发的Maven工程

6. **生命周期/插件/目标**

    1. 各个构建环节执行的顺序：不能打乱顺序，必须按照既定的正确顺序来执行
    2. Maven的核心程序中定义了抽象的生命周期，生命周期中各个阶段的具体任务是由插件来完成的
    3. Maven的核心程序是为了更好的实现自动化构建，按照这样的特点执行生命周期中的各个阶段，不论现在要执行生命周期中的哪一个阶段，都是从这个生命周期最初的位置开始执行
    4. 插件与目标
         - 生命周期的各个阶段仅仅定义了要执行的任务是什么
         - 各个阶段和插件的目标是对应的
         - 相似的目标由特定的插件来完成
         - 可以将目标看做"调用插件功能的命令"
    

7. **继承**

   需求：统一管理各个模块工程中对(例如)Junit依赖的版本
   
   解决思路：将junit遗爱统一提取到"父"工程中，在子工程中声明junit依赖时不指定版本，以父工程中统一设定的为准，同时也便于修改
   
   操作步骤
       1. 创建一个Maven工程作为父工程，注意：打包的方式pom
       2. 在子工程中声明对父工程的引用
       3. 将子工程的坐标与父工程坐标中重复的内容删除
       4. 在父工程中统一junit的依赖
       5. 在子工程中删除junit依赖的版本号部分

8. **聚合**

   作用：一键安装各个模块工程
   
   配置方式：在一个"总的聚合工程"中配置各个参与聚合的模块
   
   使用方式：在聚合工程的pom.xml上点右键->run as->maven install
 
 
