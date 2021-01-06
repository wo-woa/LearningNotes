---
title: SpringBoot笔记
date: 2020-12-23 18:14:11
tags: [SpringBoot,java,Druid,Mybatis,Shiro,Redis,Dubbo,Zokeeper]
categories: Sql
description: SpringBoot笔记
---

## 1、SpringBoot

## 回顾什么是Spring

Spring是一个开源框架，2003 年兴起的一个轻量级的Java 开发框架，作者：Rod Johnson 。

**Spring是为了解决企业级应用开发的复杂性而创建的，简化开发。**

## Spring是如何简化Java开发的

为了降低Java开发的复杂性，Spring采用了以下4种关键策略：

1、基于POJO的轻量级和最小侵入性编程，所有东西都是bean；

2、通过IOC，依赖注入（DI）和面向接口实现松耦合；

3、基于切面（AOP）和惯例进行声明式编程；

4、通过切面和模版减少样式代码，RedisTemplate，xxxTemplate；

## 什么是SpringBoot

学过javaweb的同学就知道，开发一个web应用，从最初开始接触Servlet结合Tomcat, 跑出一个Hello Wolrld程序，是要经历特别多的步骤；后来就用了框架Struts，再后来是SpringMVC，到了现在的SpringBoot，过一两年又会有其他web框架出现；你们有经历过框架不断的演进，然后自己开发项目所有的技术也在不断的变化、改造吗？建议都可以去经历一遍；

言归正传，什么是SpringBoot呢，就是一个javaweb的开发框架，和SpringMVC类似，对比其他javaweb框架的好处，官方说是简化开发，约定大于配置， you can “just run”，能迅速的开发web应用，几行代码开发一个http接口。

所有的技术框架的发展似乎都遵循了一条主线规律：从一个复杂应用场景 衍生 一种规范框架，人们只需要进行各种配置而不需要自己去实现它，这时候强大的配置功能成了优点；发展到一定程度之后，人们根据实际生产应用情况，选取其中实用功能和设计精华，重构出一些轻量级的框架；之后为了提高开发效率，嫌弃原先的各类配置过于麻烦，于是开始提倡“约定大于配置”，进而衍生出一些一站式的解决方案。

是的这就是Java企业级应用->J2EE->spring->springboot的过程。

随着 Spring 不断的发展，涉及的领域越来越多，项目整合开发需要配合各种各样的文件，慢慢变得不那么易用简单，违背了最初的理念，甚至人称配置地狱。Spring Boot 正是在这样的一个背景下被抽象出来的开发框架，目的为了让大家更容易的使用 Spring 、更容易的集成各种常用的中间件、开源软件；

Spring Boot 基于 Spring 开发，Spirng Boot 本身并不提供 Spring 框架的核心特性以及扩展功能，只是用于快速、敏捷地开发新一代基于 Spring 框架的应用程序。也就是说，它并不是用来替代 Spring 的解决方案，而是和 Spring 框架紧密结合用于提升 Spring 开发者体验的工具。Spring Boot 以**约定大于配置的核心思想**，默认帮我们进行了很多设置，多数 Spring Boot 应用只需要很少的 Spring 配置。同时它集成了大量常用的第三方库配置（例如 Redis、MongoDB、Jpa、RabbitMQ、Quartz 等等），Spring Boot 应用中这些第三方库几乎可以零配置的开箱即用。

简单来说就是SpringBoot其实不是什么新的框架，它默认配置了很多框架的使用方式，就像maven整合了所有的jar包，spring boot整合了所有的框架 。

Spring Boot 出生名门，从一开始就站在一个比较高的起点，又经过这几年的发展，生态足够完善，Spring Boot 已经当之无愧成为 Java 领域最热门的技术。

**Spring Boot的主要优点：**

- 为所有Spring开发者更快的入门
- **开箱即用**，提供各种默认配置来简化项目配置
- 内嵌式容器简化Web项目
- 没有冗余代码生成和XML配置的要求

真的很爽，我们快速去体验开发个接口的感觉吧！

# 2、第一个SpringBoot程序

## *环境准备*

- jdk1.8
- Maven 3.6.3
- Springboot:最新版
- IDEA

官方提供了一个快速生成的网站，IDEA集成了这个网站

## *项目创建方式一*

使用Spring Initializr 的 Web页面创建项目

1、打开 https://start.spring.io/

2、填写项目信息

3、点击”Generate Project“按钮生成项目；下载此项目

4、解压项目包，并用IDEA以Maven项目导入，一路下一步即可，直到项目导入完毕。

5、如果是第一次使用，可能速度会比较慢，包比较多、需要耐心等待一切就绪。

## *项目创建方式二*

使用 IDEA 直接创建项目

1、创建一个新项目

2、选择spring initalizr ， 可以看到默认就是去官网的快速构建工具那里实现

3、填写项目信息

4、选择初始化的组件（初学勾选 Web 即可）

5、填写项目路径

6、等待项目构建成功

## *项目结构分析*

通过上面步骤完成了基础项目的创建。就会自动生成以下文件。

1、程序的主启动类

2、一个 application.properties 配置文件

3、一个 测试类

4、一个 pom.xml

## *pom.xml 分析*

打开pom.xml，看看Spring Boot项目的依赖：

```xml
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>2.3.2.RELEASE</version>
  <relativePath/> <!-- lookup parent from repository -->
</parent>
<groupId>com.codekitty</groupId>
<artifactId>springboot-study</artifactId>
<version>0.0.1-SNAPSHOT</version>
<name>springboot-study</name>
<description>study project for Spring Boot</description>

<properties>
  <java.version>1.8</java.version>
</properties>

<dependencies>
  <!-- 启动器-->
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
  </dependency>
  <!-- web场景启动器 -->
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
  <!-- springboot单元测试 -->
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
    <!-- 剔除依赖 -->
    <exclusions>
      <exclusion>
        <groupId>org.junit.vintage</groupId>
        <artifactId>junit-vintage-engine</artifactId>
      </exclusion>
    </exclusions>
  </dependency>
</dependencies>
<!-- 打包插件 -->
<build>
  <plugins>
    <plugin>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-maven-plugin</artifactId>
    </plugin>
  </plugins>
</build>
```

## 启动Banner修改

在resource下建立banner.txt,放入想要的Banner图即可，Banner可以直接百度搜索springboot banner

# 3、原理初探

**自动配置：**

**pom.xml**

- Spring-boot-dependencies:核心依赖在父工程中
- 我们在写或者引入springboot依赖的时候，不需要指定版本，因为有这些版本仓库

**启动器**

- ```xml
  <!--启动器-->
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
  </dependency>
  ```

- 启动器：说白了就是Springboot的启动场景

- 比如spring-boot-starter-web，他就会帮我们自动导入web环境所有的依赖

- springboot会将所有的功能场景，都变成一个个的启动器

- 我们要使用什么功能，就值需要找到对应的启动器`starter`

**主程序**

```java
//标注这个类是一个springboot的应用
@SpringBootApplication
public class SpringbootStudyApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootStudyApplication.class, args);
    }
}
```

**注解**

- ```java
  //springboot的配置
  @SpringBootConfiguration
  
  	//spring配置类
    @Configuration 
  
  	//说明这也是一个spring的组件
    @Component	
  
  //自动配置
  @EnableAutoConfiguration
  
  	//自动配置包
  	@AutoConfigurationPackage 
  
  	//自动配置	
  		@Import(AutoConfigurationPackages.Registrar.class)
  
  	//自动配置导入选择
  	@Import(AutoConfigurationImportSelector.class)
  
  //获取所有的配置
  List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes);
  ```

  获取候选的配置

  ```java
  protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
    List<String> configurations = SpringFactoriesLoader.loadFactoryNames(getSpringFactoriesLoaderFactoryClass(),
                                                                         getBeanClassLoader());
    Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you "
                    + "are using a custom packaging, make sure that file is correct.");
    return configurations;
  }
  ```

  META-INF/spring.factories.

Springboot所有自动配置都是在启动的时候扫描并加载：`spring.factories`所有的自动配置类都在这里面，但是不一定生效，要判断条件是否成立，只要导入了对应的strater，就有了对应的启动器，有了启动器，我们自动装配就会生效，然后配置成功

1. springboot在启动的时候，从类路径下 /META-INF/spring.factories.获取指定的值；

![在这里插入图片描述](https://gitee.com/wowoa/typoraPic/raw/master/image2020/20201229144115.jpg)

1. 将这些自动配置的类导入容器，自动配置就会生效，帮我们进行自动配置
2. 以前我们需要手动配置的东西，现在Springboot帮我们做了
3. 整合javaEE，解决方案和自动配置的东西都在spring-boot-autoconfigure包下
4. 它会把所有需要导入的组件，以类名的方式返回，这些组件就会添加到容器
5. 容器中也会存在XXXXautoConfiguration的文件(@Bean)，就是这些类给容器中导入了这个场景所需要的所有组件,并自动配置，@Configuration,JavaConfig
6. 有了自动配置类，免去了我们自己编写配置文件的步骤

**启动原理：**

 最初以为就是运行了一个main方法，没想到却开启了一个服务；

```java
@SpringBootApplication
public class SpringbootApplication {
    public static void main(String[] args) {
        SpringApplication.run(SpringbootApplication.class, args);
    }
}
```

**这个类主要做了以下四件事情：**

1、推断应用的类型是普通的项目还是Web项目

2、查找并加载所有可用初始化器 ， 设置到initializers属性中

3、找出所有的应用程序监听器，设置到listeners属性中

4、推断并设置main方法的定义类，找到运行的主类

**run方法流程分析**

![img](https://gitee.com/wowoa/typoraPic/raw/master/image2020/20201229144116.jpg)

# 4、Springboot配置文件

SpringBoot使用一个全局的配置文件 ， 配置文件名称是固定的

- application.properties

- - 语法结构 ：key=value

    ```xml
    server.port=8081
    ```

- application.yml

- - 语法结构 ：key：空格 value

    ```xml
    server:
      port: 8081
    ```

- application.yml

  - 语法结构 ：key：空格 value

    ```yaml
    server:
      port: 8081
    # 普通的key-value
    name: 李明
    
    #对象
    student:
      name: jj
      age: 3
    
    #行内写法
    student1: {name: jj,age: 3}
    
    #数组
    pets:
      - cat
      - dog
      - pig
    
    pets1: [cat,dog,pig]
    ```

**yaml可以直接给实体类赋值**

在yaml写好配置

```yaml
person:
  name: 小红
  age: 3
  happy: true
  birth: 2020/7/27
  maps: {k1: v1,k2: v2}
  lists:
    - code
    - music
  dog:
    name: 旺旺
    age: 3
```

在实体类上加上注解

```java
@ConfigurationProperties(prefix = "person")
```

在Springboot提供的单元测试类中测试

```java
@Autowired
private Person person;
@Test
void contextLoads() {
  System.out.println(person);
}
```

注意：注解的prefix值要和配置文件中的属性名相同

**yaml的松散绑定**

 比如我的yaml中写的last-name，这个和lastName是一样的， - 后面跟着的字母默认是大写的。这就是松散绑定。

**结论：**

- 配置yml和配置properties都可以获取到值 ， 强烈推荐 yml；
- 如果我们在某个业务中，只需要获取配置文件中的某个值，可以使用一下 @value；
- 如果说，我们专门编写了一个JavaBean来和配置文件进行一一映射，就直接@configurationProperties，不要犹豫！

**JSR303数据校验**

 这个就是我们可以在字段是增加一层过滤器验证 ， 可以保证数据的合法性

***使用***

1. 在实体类上增加`@Validated`注解

2. 在实体属性上增加以下注解即可实现相应的校验,在注解内加入`message`属性可以指定校验未通过时的提示信息

   ```makefile
   空检查 
   @Null 验证对象是否为null 
   @NotNull 验证对象是否不为null, 无法查检长度为0的字符串 
   @NotBlank 检查约束字符串是不是Null还有被Trim的长度是否大于0,只对字符串,且会去掉前后空格. 
   @NotEmpty 检查约束元素是否为NULL或者是EMPTY.
   
   Booelan检查 
   @AssertTrue 验证 Boolean 对象是否为 true 
   @AssertFalse 验证 Boolean 对象是否为 false
   
   长度检查 
   @Size(min=, max=) 验证对象（Array,Collection,Map,String）长度是否在给定的范围之内 
   @Length(min=, max=) Validates that the annotated string is between min and max included.
   
   日期检查 
   @Past 验证 Date 和 Calendar 对象是否在当前时间之前，验证成立的话被注释的元素一定是一个过去的日期 
   @Future 验证 Date 和 Calendar 对象是否在当前时间之后 ，验证成立的话被注释的元素一定是一个将来的日期 
   @Pattern 验证 String 对象是否符合正则表达式的规则，被注释的元素符合制定的正则表达式，regexp:正则表达式 flags: 指定 Pattern.Flag 的数组，表示正则表达式的相关选项。
   
   数值检查 
   建议使用在Stirng,Integer类型，不建议使用在int类型上，因为表单值为“”时无法转换为int，但可以转换为Stirng为”“,Integer为null 
   @Min 验证 Number 和 String 对象是否大等于指定的值 
   @Max 验证 Number 和 String 对象是否小等于指定的值 
   @DecimalMax 被标注的值必须不大于约束中指定的最大值. 这个约束的参数是一个通过BigDecimal定义的最大值的字符串表示.小数存在精度 
   @DecimalMin 被标注的值必须不小于约束中指定的最小值. 这个约束的参数是一个通过BigDecimal定义的最小值的字符串表示.小数存在精度 
   @Digits 验证 Number 和 String 的构成是否合法 
   @Digits(integer=,fraction=) 验证字符串是否是符合指定格式的数字，interger指定整数精度，fraction指定小数精度。 
   @Range(min=, max=) 被指定的元素必须在合适的范围内 
   @Range(min=10000,max=50000,message=”range.bean.wage”) 
   @Valid 递归的对关联对象进行校验, 如果关联对象是个集合或者数组,那么对其中的元素进行递归校验,如果是一个map,则对其中的值部分进行校验.(是否进行递归验证) 
   @CreditCardNumber信用卡验证 
   @Email 验证是否是邮件地址，如果为null,不进行验证，算通过验证。 
   @ScriptAssert(lang= ,script=, alias=) 
   @URL(protocol=,host=, port=,regexp=, flags=)
   ```

 数据校验的注解存储在Maven: jakarta.validation:jakarta.validation-api:2.0.2包内[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-iC4c9seR-1599793610943)(/Users/mac/Desktop/截屏2020-07-27 上午9.23.59.png)]

**可以在多个位置使用配置文件**

配置文件的优先级由高到低分别为项目路径下的config/application.yaml>application.yaml>resources/config/application.yaml>resources/application.yaml

默认创建的是优先级最低的resources/application.yaml

**多环境切换**

yaml多环境切换

用`---`来分隔多个文件，使用profiles的active来选择调用哪个环境

```yaml
server:
  port: 8081
spring:
  profiles:
    active: dev
---
server:
  port: 8082
spring:
  profiles: dev

---
server:
  port: 8083
spring:
  profiles: test
```

**自动配置的原理！**

1、SpringBoot启动会加载大量的自动配置类

2、我们看我们需要的功能有没有在SpringBoot默认写好的自动配置类当中；

3、我们再来看这个自动配置类中到底配置了哪些组件；（只要我们要用的组件存在在其中，我们就不需要再手动配置了）

4、给容器中自动配置类添加组件的时候，会从properties类中获取某些属性。我们只需要在配置文件中指定这些属性的值即可；

**xxxxAutoConfigurartion：自动配置类；**给容器中添加组件

**xxxxProperties:封装配置文件中相关属性；**

开启debug可以在启动的时候在控制台输出配置

```yaml
debug: true
```

# 5、SpringBoot Web开发

Jar: webapp

自动装配

springboot到底帮我们配置了什么？能不能修改，能修改哪些东西？能不能拓展

- xxxxAutoConfigurartion…向容器中自动配置组件
- xxxxProperties:自动配置类，装配配置文件中自定义的一些内容

要解决的问题:

- 导入静态资源…
- 首页
- jsp,模版引擎Thymeleaf
- 装配扩展SpringMVC
- 增删改查
- 拦截器
- 国际化

**静态资源总结：**

1. 在springboot，我们 可以使用以下方式处理静态资源
   - webjars`localhost:8080/wbjars/`
   - Public, static,/**,resources, `localhost:8080`
2. 优先级：resources>static>(默认)>public

# 6、Thymeleaf模版引擎

结论：只要导入对应的maven依赖即可使用

```xml
<!--thymeleaf-->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

2、我们要使用thymeleaf，需要在html文件中导入命名空间的约束，方便提示,我们可以去官方文档。

```html
xmlns:th="http://www.thymeleaf.org"
```

3、在controller设置接收请求

```java
@GetMapping("/test")
public String test(Model model){
  model.addAttribute("msg","helloSpringboot");
  return "test";
}
```

4、在主入口启动，访问`localhost:8080/test`即可

**注意：在项目新建module是无法访问到，最终找到原因是创建module时没有导入thymeleaf**

### 遍历数组

用th:each遍历，有两种取值方法

```html
<h3 th:each="user:${users}" th:text="${user}"></h3>
<h3 th:each="user:${users}">[[${user}}]]</h3>
```

### **Thymeleaf语法**

![img](https://gitee.com/wowoa/typoraPic/raw/master/image2020/20201229144117.jpg)

### Thymeleaf表达式

```
Simple expressions:（表达式语法）
Variable Expressions: ${...}：获取变量值；OGNL；
    1）、获取对象的属性、调用方法
    2）、使用内置的基本对象：#18
         #ctx : the context object.
         #vars: the context variables.
         #locale : the context locale.
         #request : (only in Web Contexts) the HttpServletRequest object.
         #response : (only in Web Contexts) the HttpServletResponse object.
         #session : (only in Web Contexts) the HttpSession object.
         #servletContext : (only in Web Contexts) the ServletContext object.

    3）、内置的一些工具对象：
　　　　　　#execInfo : information about the template being processed.
　　　　　　#uris : methods for escaping parts of URLs/URIs
　　　　　　#conversions : methods for executing the configured conversion service (if any).
　　　　　　#dates : methods for java.util.Date objects: formatting, component extraction, etc.
　　　　　　#calendars : analogous to #dates , but for java.util.Calendar objects.
　　　　　　#numbers : methods for formatting numeric objects.
　　　　　　#strings : methods for String objects: contains, startsWith, prepending/appending, etc.
　　　　　　#objects : methods for objects in general.
　　　　　　#bools : methods for boolean evaluation.
　　　　　　#arrays : methods for arrays.
　　　　　　#lists : methods for lists.
　　　　　　#sets : methods for sets.
　　　　　　#maps : methods for maps.
　　　　　　#aggregates : methods for creating aggregates on arrays or collections.
==================================================================================
Selection Variable Expressions: *{...}：选择表达式：和${}在功能上是一样；
Message Expressions: #{...}：获取国际化内容
Link URL Expressions: @{...}：定义URL；
Fragment Expressions: ~{...}：片段引用表达式

Literals（字面量）
Text literals: 'one text' , 'Another one!' ,…
Number literals: 0 , 34 , 3.0 , 12.3 ,…
Boolean literals: true , false
Null literal: null
Literal tokens: one , sometext , main ,…

Text operations:（文本操作）
String concatenation: +
Literal substitutions: |The name is ${name}|

Arithmetic operations:（数学运算）
Binary operators: + , - , * , / , %
Minus sign (unary operator): -

Boolean operations:（布尔运算）
Binary operators: and , or
Boolean negation (unary operator): ! , not

Comparisons and equality:（比较运算）
Comparators: > , < , >= , <= ( gt , lt , ge , le )
Equality operators: == , != ( eq , ne )

Conditional operators:条件运算（三元运算符）
If-then: (if) ? (then)
If-then-else: (if) ? (then) : (else)
Default: (value) ?: (defaultvalue)

Special tokens:
No-Operation: _
```

# 7、装配MVC

官方建议：直接创建一个MVCconfig类，在类上加上`@Configuration`注解，并且实现`WebMvcConfigurer`接口，并且不能使用`@EnableWebMvc`注解

**为什么不能使用`@EnableWebMvc`注解**

- 这个注解导入了一个类：DelegatingWebMvcConfiguration,这个类从容器中获取所有的webmvcconfig

- 并且在WebMvcAutoConfiguration类中有这样一个注解：

  ```java
  @ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
  ```

- 这个注解的意思就是：容器中没有这个组件的时候，这个自动配置类才生效

- 如果加了`@EnableWebMVC`容器中就有了组件，这个配置就不生效了

**如果需要全面接管SpringMVC可以使用该注解，当然在开发中，不推荐使用全面接管SpringMVC**

# 8、进行项目前的准备

1. 准备好数据，在这里用Map模拟数据库中的数据，后期再进行数据库整合

   ```java
   @Repository
   public class DeptDao {
     private static Map<Integer, Dept> depts = null;
     static{
       //模拟数据库中的数据
       depts = new HashMap<Integer,Dept>();//创建一个部门表
       depts.put(101,new Dept(101,"教学部"));
       depts.put(102,new Dept(102,"市场部"));
       depts.put(103,new Dept(103,"后勤部"));
       depts.put(104,new Dept(104,"教研部"));
     }
     //获得所有部门信息
     public Collection<Dept> getDept(){
       return depts.values();
     }
     public Dept getDept(Integer id){
       return depts.get(id);
     }
   }
   ```

2. 准备好模版（在网上找bootstrap或其他的模版，或者自己写）

3. 页面直接放在templates下，css、img、js等放在static下

4. 修改html页面，使其符合Thymeleaf模版规范

   - 在url路径属性前增加`th:`并修改url路径为`@{}`格式（js、css、img等）

# 9、项目：国际化

1. 在resources下创建`i18n`文件夹，并创建`login.proterties`文件`login_zh_CN.proterties`文件`login_en_US.proterties`文件

2. 在内部写入配置

   ```properties
   login.password=password
   login.tip=Please sign in
   login.username=username
   #------------------------
   login.password=密码
   login.tip=请登录
   login.username=用户名
   #------------------------
   login.password=password
   login.tip=Please sign in
   login.username=username
   #------------------------
   ```

3. 在核心配置文件中配置一下属性

   ```properties
   spring.messages.basename=i18n.login
   ```

4. 在config包内创建类，实现`localereslover`接口,重写方法,解析请求

   ```java
    //解析请求
       @Override
       public Locale resolveLocale(HttpServletRequest request) {
           String language = request.getParameter("l");
           Locale locale = Locale.getDefault();//得到一个默认的，如果没有传入的就使用默认的
           System.out.println("==========>" + language);
           if(!StringUtils.isEmpty(language)){
               String[] s = language.split("_");
               System.out.println("----------------");
               locale = new Locale(s[0], s[1]);
           }
           return locale;
       }
   ```

5. 在mvcconfig配置Bean

   ```java
   @Bean
       public LocaleResolver localeResolver(){
           return new MyLocaleResolver();
       }
   ```

6. 在页面设置按钮发送请求，并修改页面文字元素为`thymeleaf`格式

   ```html
   <a class="btn btn-sm" th:href="@{/index.html(l='zh_CN')}">中文</a>|
   <a class="btn btn-sm" th:href="@{/index.html(l='en_US')}">English</a>
   ```

7. 测试运行

# 10、项目：登陆 + 拦截器

1. 修改页面form的action请求action为`th:action`

2. 新建controller

   ```java
   @Controller
   public class LoginController {
     @RequestMapping("/login")
     public String Login(@RequestParam("userName") String userName,
                         @RequestParam("passWord") String passWord,
                         Model model, HttpSession session){
       if(!StringUtils.isEmpty(userName) && "12345".equals(passWord)){
         session.setAttribute("loginUser",userName);
    System.out.println(session.getAttribute("loginUser"));
         return "redirect:/main.html";
       }else{
         model.addAttribute("msg","用户名或密码错误");
         System.out.println("444");
         return "login";
       }
     }
   }
   ```

   **注意：在这里将userName存入了session中，取的时候需要通过session.userName来取**

# 11、展示员工页面

1. 首先做页面代码复用工作，取页面公共组件放在一个页面中，每个组件设置一个`th:fragment=""`属性，在需要用的页面使用`th:insert`或者`th:replace`属性进行复用

   ```html
   <div th:replace="~{empl/list::top}"></div>
   ```

2. 定义一个controller类来实现处理请求数据的功能

   ```java
   @Controller
   public class EmployeeController {
     @Autowired
     EmplDao empl;
     @RequestMapping("/empl")
     public String list(Model model){
       Collection<Empl> all = empl.getAll();
       model.addAttribute("emplList",all);
       for (Empl empl : all) {
         System.out.println(empl.toString());
       }
       return "empl/list";
     }
   }
   ```

3. 在页面中定义表格，将获取到的数据通过thymeleaf显示出来

   ```html
   <table class="table table-bordered">
     <thead>
       <tr>
         <th>id</th>
         <th>lastName</th>
         <th>email</th>
         <th>gender</th>
         <th>deptName</th>
         <th>birth</th>
         <th>操作</th>
       </tr>
     </thead>
     <tbody>
       <tr th:each="emp:${emplList}">
   
         <td th:text="${emp.getId()}"></td>
         <td th:text="${emp.getLastName()}"></td>
         <td th:text="${emp.getEmail()}"></td>
         <td th:text="${emp.getGender()==0?'女':'男'}"></td>
         <td th:text="${emp.getDeptName().getDeptName()}"></td>
         <td th:text="${#dates.format(emp.getBirth())}"></td>
         <td>
           <button class="btn btn-sm btn-primary" >编辑</button>
           <button class="btn btn-sm btn-danger" >删除</button>
         </td>
       </tr>
     </tbody>
   </table>
   ```

- 本节遇到的问题
  - 点击请求数据时出现500错误，原因是controller没有加`@Autowired`注解
  - 数据显示时只显示一行，并且是最后一条数据，开始以为是显示的原因，后来向上查找原因发现是使用Map代替数据库时，map的键重复，导致map的put时后面会覆盖前面定义好的，最终只剩一条最后插入的数据（我贼智障）

# 回顾

- SpringBoot是什么
- 微服务
- HelloWorld
- 探究源码～自动装配原理
- 配置yaml
- 多文档环境切换
- 静态资源映射
- Thymeleaf th:xxx
- SpringBoot 如何扩展MVC javaConfig～
- 如何修改SpringBoot的默认配置
- CRUD
- 国际化
- 拦截器
- 定制首页，错误页

# 接下来

- JDBC
- **Mybatis**
- **Druid**
- **Shiro:安全**
- **Spring Security:安全**
- 异步任务～，邮件发送，定时任务
- Swagger
- **Dubbo + Zookeeper**

# 12、整合JDBC

1. 新建项目，引入`JDBC API`、`mysql Driver`和`Spring Web`

Springboot会自动帮我们导入以下启动器

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
```

1. 编写yaml配置文件

```yaml
spring:
  datasource:
    username: codekitty
    password: lihaiyang
    url: jdbc:mysql://codekitty.cn:3306/study?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
    driver-class-name: com.mysql.cj.jdbc.Driver
```

1. 可以使用了，进行测试

   ```java
   //DI注入数据源
   @Autowired
   DataSource dataSource;
   @Test
   void contextLoads() throws SQLException {
     //查看一下数据源
     System.out.println(dataSource.getClass());
   
     //获得数据库连接
     Connection connection = dataSource.getConnection();
     System.out.println(connection);
   	//关闭连接
     connection.close();
   }
   ```

结果：我们可以看到他默认给我们配置的数据源为 : class com.zaxxer.hikari.HikariDataSource ， 我们并没有手动配置

我们来全局搜索一下，找到数据源的所有自动配置都在 ：DataSourceAutoConfiguration文件：

```java
@Import(    {Hikari.class, Tomcat.class, Dbcp2.class, Generic.class, DataSourceJmxConfiguration.class})protected static class PooledDataSourceConfiguration {    protected PooledDataSourceConfiguration() {    }}
```

这里导入的类都在 DataSourceConfiguration 配置类下，可以看出 Spring Boot 2.2.5 默认使用HikariDataSource 数据源，而以前版本，如 Spring Boot 1.5 默认使用 org.apache.tomcat.jdbc.pool.DataSource 作为数据源；

**HikariDataSource 号称 Java WEB 当前速度最快的数据源，相比于传统的 C3P0 、DBCP、Tomcat jdbc 等连接池更加优秀；**

**可以使用 spring.datasource.type 指定自定义的数据源类型，值为 要使用的连接池实现的完全限定名。**

关于数据源我们并不做介绍，有了数据库连接，显然就可以 CRUD 操作数据库了。但是我们需要先了解一个对象 JdbcTemplate

## JDBCTemplate

1、有了数据源(com.zaxxer.hikari.HikariDataSource)，然后可以拿到数据库连接(java.sql.Connection)，有了连接，就可以使用原生的 JDBC 语句来操作数据库；

2、即使不使用第三方第数据库操作框架，如 MyBatis等，Spring 本身也对原生的JDBC 做了轻量级的封装，即JdbcTemplate。

3、数据库操作的所有 CRUD 方法都在 JdbcTemplate 中。

4、Spring Boot 不仅提供了默认的数据源，同时默认已经配置好了 JdbcTemplate 放在了容器中，程序员只需自己注入即可使用

5、JdbcTemplate 的自动配置是依赖 org.springframework.boot.autoconfigure.jdbc 包下的 JdbcTemplateConfiguration 类

**JdbcTemplate主要提供以下几类方法：**

- execute方法：可以用于执行任何SQL语句，一般用于执行DDL语句；
- update方法及batchUpdate方法：update方法用于执行新增、修改、删除等语句；batchUpdate方法用于执行批处理相关语句；
- query方法及queryForXXX方法：用于执行查询相关语句；
- call方法：用于执行存储过程、函数相关语句。

**测试**

编写一个controller，注入jdbcTemplate，编写测试方法进行访问测试

```java
@RestController
public class JDBCcontroller {
  @Autowired
  JdbcTemplate jdbcTemplate;

  //查询数据库的所有信息
  //List中的1个Map对应数据库中的一行数据
  //Map中的key对应数据库中的字段名，value对应数据库的字段值
  @GetMapping("/userList")
  public List<Map<String,Object>> userList(){
    String sql = "select * from User";
    List<Map<String, Object>> maps = jdbcTemplate.queryForList(sql);
    return maps;
  }
  //新增一个用户
  @GetMapping("/addUser")
  public String addUser(){
    String sql = "insert into study.User(id,username,userPassword) values(10,'李华','09876543456')";
    jdbcTemplate.update(sql);
    return "update-ok";
  }
  //修改一个用户
  @GetMapping("/updateUser/{id}")
  public String updateUser(@PathVariable("id") int id){
    //插入语句
    String sql = "update study.User set username=?,userPassword=? where id=" + id;
		//数据
    Object[] objects = new Object[2];
    objects[0] = "李华2";
    objects[1] = "3333333";
    jdbcTemplate.update(sql,objects);
    return "update-ok";
  }
  //删除用户
  @GetMapping("/deleteUser/{id}")
  public String deleteUser(@PathVariable("id") int id){
    String sql = "delete from study.User where id=" + id;
    jdbcTemplate.update(sql);
    return "update-ok";
  }
}
```

# 13、整合Druid

## Druid简介

Java程序很大一部分要操作数据库，为了提高性能操作数据库的时候，又不得不使用数据库连接池。

Druid 是阿里巴巴开源平台上一个数据库连接池实现，结合了 C3P0、DBCP 等 DB 池的优点，同时加入了日志监控。

Druid 可以很好的监控 DB 池连接和 SQL 的执行情况，天生就是针对监控而生的 DB 连接池。

Druid已经在阿里巴巴部署了超过600个应用，经过一年多生产环境大规模部署的严苛考验。

Spring Boot 2.0 以上默认使用 Hikari 数据源，可以说 Hikari 与 Driud 都是当前 Java Web 上最优秀的数据源，我们来重点介绍 Spring Boot 如何集成 Druid 数据源，如何实现数据库监控。

Github地址：https://github.com/alibaba/druid/

**基本配置**

| 配置                          | 缺省值             | 说明                                                         |
| ----------------------------- | ------------------ | ------------------------------------------------------------ |
| name                          |                    | 配置这个属性的意义在于，如果存在多个数据源，监控的时候可以通过名字来区分开来。 如果没有配置，将会生成一个名字，格式是：“DataSource-” + System.identityHashCode(this) |
| jdbcUrl                       |                    | 连接数据库的url，不同数据库不一样。例如： mysql : jdbc:mysql://10.20.153.104:3306/druid2 oracle : jdbc:oracle:thin:@10.20.149.85:1521:ocnauto |
| username                      |                    | 连接数据库的用户名                                           |
| password                      |                    | 连接数据库的密码。如果你不希望密码直接写在配置文件中，可以使用ConfigFilter。详细看这里：https://github.com/alibaba/druid/wiki/%E4%BD%BF%E7%94%A8ConfigFilter |
| driverClassName               | 根据url自动识别    | 这一项可配可不配，如果不配置druid会根据url自动识别dbType，然后选择相应的driverClassName(建议配置下) |
| initialSize                   | 0                  | 初始化时建立物理连接的个数。初始化发生在显示调用init方法，或者第一次getConnection时 |
| maxActive                     | 8                  | 最大连接池数量                                               |
| maxIdle                       | 8                  | 已经不再使用，配置了也没效果                                 |
| minIdle                       |                    | 最小连接池数量                                               |
| maxWait                       |                    | 获取连接时最大等待时间，单位毫秒。配置了maxWait之后，缺省启用公平锁，并发效率会有所下降，如果需要可以通过配置useUnfairLock属性为true使用非公平锁。 |
| poolPreparedStatements        | false              | 是否缓存preparedStatement，也就是PSCache。PSCache对支持游标的数据库性能提升巨大，比如说oracle。在mysql下建议关闭。 |
| maxOpenPreparedStatements     | -1                 | 要启用PSCache，必须配置大于0，当大于0时，poolPreparedStatements自动触发修改为true。在Druid中，不会存在Oracle下PSCache占用内存过多的问题，可以把这个数值配置大一些，比如说100 |
| validationQuery               |                    | 用来检测连接是否有效的sql，要求是一个查询语句。如果validationQuery为null，testOnBorrow、testOnReturn、testWhileIdle都不会其作用。 |
| testOnBorrow                  | true               | 申请连接时执行validationQuery检测连接是否有效，做了这个配置会降低性能。 |
| testOnReturn                  | false              | 归还连接时执行validationQuery检测连接是否有效，做了这个配置会降低性能 |
| testWhileIdle                 | false              | 建议配置为true，不影响性能，并且保证安全性。申请连接的时候检测，如果空闲时间大于timeBetweenEvictionRunsMillis，执行validationQuery检测连接是否有效。 |
| timeBetweenEvictionRunsMillis |                    | 有两个含义： 1) Destroy线程会检测连接的间隔时间2) testWhileIdle的判断依据，详细看testWhileIdle属性的说明 |
| numTestsPerEvictionRun        |                    | 不再使用，一个DruidDataSource只支持一个EvictionRun           |
| minEvictableIdleTimeMillis    |                    |                                                              |
| connectionInitSqls            |                    | 物理连接初始化的时候执行的sql                                |
| exceptionSorter               | 根据dbType自动识别 | 当数据库抛出一些不可恢复的异常时，抛弃连接                   |
| filters                       |                    | 属性类型是字符串，通过别名的方式配置扩展插件，常用的插件有： 监控统计用的filter:stat日志用的filter:log4j防御sql注入的filter:wall |
| proxyFilters                  |                    | 类型是List<com.alibaba.druid.filter.Filter>，如果同时配置了filters和proxyFilters，是组合关系，并非替换关系 |

| 配置                          | 缺省值             | 说明                                                         |
| ----------------------------- | ------------------ | ------------------------------------------------------------ |
| name                          |                    | 配置这个属性的意义在于，如果存在多个数据源，监控的时候可以通过名字来区分开来。 如果没有配置，将会生成一个名字，格式是：“DataSource-” + System.identityHashCode(this) |
| jdbcUrl                       |                    | 连接数据库的url，不同数据库不一样。例如： mysql : jdbc:mysql://10.20.153.104:3306/druid2 oracle : jdbc:oracle:thin:@10.20.149.85:1521:ocnauto |
| username                      |                    | 连接数据库的用户名                                           |
| password                      |                    | 连接数据库的密码。如果你不希望密码直接写在配置文件中，可以使用ConfigFilter。详细看这里：https://github.com/alibaba/druid/wiki/%E4%BD%BF%E7%94%A8ConfigFilter |
| driverClassName               | 根据url自动识别    | 这一项可配可不配，如果不配置druid会根据url自动识别dbType，然后选择相应的driverClassName(建议配置下) |
| initialSize                   | 0                  | 初始化时建立物理连接的个数。初始化发生在显示调用init方法，或者第一次getConnection时 |
| maxActive                     | 8                  | 最大连接池数量                                               |
| maxIdle                       | 8                  | 已经不再使用，配置了也没效果                                 |
| minIdle                       |                    | 最小连接池数量                                               |
| maxWait                       |                    | 获取连接时最大等待时间，单位毫秒。配置了maxWait之后，缺省启用公平锁，并发效率会有所下降，如果需要可以通过配置useUnfairLock属性为true使用非公平锁。 |
| poolPreparedStatements        | false              | 是否缓存preparedStatement，也就是PSCache。PSCache对支持游标的数据库性能提升巨大，比如说oracle。在mysql下建议关闭。 |
| maxOpenPreparedStatements     | -1                 | 要启用PSCache，必须配置大于0，当大于0时，poolPreparedStatements自动触发修改为true。在Druid中，不会存在Oracle下PSCache占用内存过多的问题，可以把这个数值配置大一些，比如说100 |
| validationQuery               |                    | 用来检测连接是否有效的sql，要求是一个查询语句。如果validationQuery为null，testOnBorrow、testOnReturn、testWhileIdle都不会其作用。 |
| testOnBorrow                  | true               | 申请连接时执行validationQuery检测连接是否有效，做了这个配置会降低性能。 |
| testOnReturn                  | false              | 归还连接时执行validationQuery检测连接是否有效，做了这个配置会降低性能 |
| testWhileIdle                 | false              | 建议配置为true，不影响性能，并且保证安全性。申请连接的时候检测，如果空闲时间大于timeBetweenEvictionRunsMillis，执行validationQuery检测连接是否有效。 |
| timeBetweenEvictionRunsMillis |                    | 有两个含义： 1) Destroy线程会检测连接的间隔时间2) testWhileIdle的判断依据，详细看testWhileIdle属性的说明 |
| numTestsPerEvictionRun        |                    | 不再使用，一个DruidDataSource只支持一个EvictionRun           |
| minEvictableIdleTimeMillis    |                    |                                                              |
| connectionInitSqls            |                    | 物理连接初始化的时候执行的sql                                |
| exceptionSorter               | 根据dbType自动识别 | 当数据库抛出一些不可恢复的异常时，抛弃连接                   |
| filters                       |                    | 属性类型是字符串，通过别名的方式配置扩展插件，常用的插件有： 监控统计用的filter:stat日志用的filter:log4j防御sql注入的filter:wall |
| proxyFilters                  |                    | 类型是List<com.alibaba.druid.filter.Filter>，如果同时配置了filters和proxyFilters，是组合关系，并非替换关系 |

| 配置                          | 缺省值             | 说明                                                         |
| ----------------------------- | ------------------ | ------------------------------------------------------------ |
| name                          |                    | 配置这个属性的意义在于，如果存在多个数据源，监控的时候可以通过名字来区分开来。 如果没有配置，将会生成一个名字，格式是：“DataSource-” + System.identityHashCode(this) |
| jdbcUrl                       |                    | 连接数据库的url，不同数据库不一样。例如： mysql : jdbc:mysql://10.20.153.104:3306/druid2 oracle : jdbc:oracle:thin:@10.20.149.85:1521:ocnauto |
| username                      |                    | 连接数据库的用户名                                           |
| password                      |                    | 连接数据库的密码。如果你不希望密码直接写在配置文件中，可以使用ConfigFilter。详细看这里：https://github.com/alibaba/druid/wiki/%E4%BD%BF%E7%94%A8ConfigFilter |
| driverClassName               | 根据url自动识别    | 这一项可配可不配，如果不配置druid会根据url自动识别dbType，然后选择相应的driverClassName(建议配置下) |
| initialSize                   | 0                  | 初始化时建立物理连接的个数。初始化发生在显示调用init方法，或者第一次getConnection时 |
| maxActive                     | 8                  | 最大连接池数量                                               |
| maxIdle                       | 8                  | 已经不再使用，配置了也没效果                                 |
| minIdle                       |                    | 最小连接池数量                                               |
| maxWait                       |                    | 获取连接时最大等待时间，单位毫秒。配置了maxWait之后，缺省启用公平锁，并发效率会有所下降，如果需要可以通过配置useUnfairLock属性为true使用非公平锁。 |
| poolPreparedStatements        | false              | 是否缓存preparedStatement，也就是PSCache。PSCache对支持游标的数据库性能提升巨大，比如说oracle。在mysql下建议关闭。 |
| maxOpenPreparedStatements     | -1                 | 要启用PSCache，必须配置大于0，当大于0时，poolPreparedStatements自动触发修改为true。在Druid中，不会存在Oracle下PSCache占用内存过多的问题，可以把这个数值配置大一些，比如说100 |
| validationQuery               |                    | 用来检测连接是否有效的sql，要求是一个查询语句。如果validationQuery为null，testOnBorrow、testOnReturn、testWhileIdle都不会其作用。 |
| testOnBorrow                  | true               | 申请连接时执行validationQuery检测连接是否有效，做了这个配置会降低性能。 |
| testOnReturn                  | false              | 归还连接时执行validationQuery检测连接是否有效，做了这个配置会降低性能 |
| testWhileIdle                 | false              | 建议配置为true，不影响性能，并且保证安全性。申请连接的时候检测，如果空闲时间大于timeBetweenEvictionRunsMillis，执行validationQuery检测连接是否有效。 |
| timeBetweenEvictionRunsMillis |                    | 有两个含义： 1) Destroy线程会检测连接的间隔时间2) testWhileIdle的判断依据，详细看testWhileIdle属性的说明 |
| numTestsPerEvictionRun        |                    | 不再使用，一个DruidDataSource只支持一个EvictionRun           |
| minEvictableIdleTimeMillis    |                    |                                                              |
| connectionInitSqls            |                    | 物理连接初始化的时候执行的sql                                |
| exceptionSorter               | 根据dbType自动识别 | 当数据库抛出一些不可恢复的异常时，抛弃连接                   |
| filters                       |                    | 属性类型是字符串，通过别名的方式配置扩展插件，常用的插件有： 监控统计用的filter:stat日志用的filter:log4j防御sql注入的filter:wall |
| proxyFilters                  |                    | 类型是List<com.alibaba.druid.filter.Filter>，如果同时配置了filters和proxyFilters，是组合关系，并非替换关系 |

1、添加上 Druid 数据源依赖。

```xml
<!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
<dependency>    
  <groupId>com.alibaba</groupId>
  <artifactId>druid</artifactId>   
  <version>1.1.21</version>
</dependency>
```

2、切换数据源；springBoot2.0以上默认使用，但可以通过spring.datasource.type指定数据源

```yaml
spring:
  datasource:
    username: codekitty
    password: lihaiyang
    url: jdbc:mysql://codekitty.cn:3306/study?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource # 自定义数据源

    #Spring Boot 默认是不注入这些属性值的，需要自己绑定
    #druid 数据源专有配置
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true

    #配置监控统计拦截的filters，stat:监控统计、log4j：日志记录、wall：防御sql注入
    #如果允许时报错  java.lang.ClassNotFoundException: org.apache.log4j.Priority
    #则导入 log4j 依赖即可，Maven 地址：https://mvnrepository.com/artifact/log4j/log4j
    filters: stat,wall,log4j
    maxPoolPreparedStatementPerConnectionSize: 20
    useGlobalDataSourceStat: true
    connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
```

## 进行测试

```java
@Configuration
public class DruidConfig {
  @Bean
  @ConfigurationProperties(prefix = "spring.datasource")
  public DataSource druidDataSource(){

    return new DruidDataSource();
  }

  @Bean
  //后台监控功能
  //因为SpringBoot内置了Servlet容器，所以没有Web.xml，替代方法：ServletRegistrationBean
  public ServletRegistrationBean statViewServlet(){

    ServletRegistrationBean bean = new ServletRegistrationBean<>(new StatViewServlet(),"/druid/*");
    //后台需要有人登陆，账号密码配置
    HashMap<String,String> map = new HashMap<>();
    //增加配置
    map.put("loginUsername","admin");//登陆的key，是固定的，不能自己定义成其他的
    map.put("loginPassword","123456");

    //允许谁可以访问
    map.put("allow","localhost");

    bean.setInitParameters(map);//初始化参数
    return bean;
  }
  @Bean
  //filter
  public FilterRegistrationBean WebStartFilter(){
    FilterRegistrationBean bean = new FilterRegistrationBean<>();
    //设置过滤器
    bean.setFilter(new WebStatFilter());
    //可以过滤哪些请求
    Map<String,String> filters = new HashMap<>();
    //这些东西不进行统计
    filters.put("exclusions","*.js,*.css,/druid/*");
    bean.setInitParameters(filters);

    return bean;
  }
}
```

# 14、整合Mybatis

整合包

mybatis-spring-boot-starter

1. 导入包

   ```xml
   <dependency>
     <groupId>org.mybatis.spring.boot</groupId>
     <artifactId>mybatis-spring-boot-starter</artifactId>
     <version>2.1.1</version>
   </dependency>
   ```

2. 配置文件

   ```yaml
   #整合mybatis
   mybatis:
     type-aliases-package: com.springbootdatamybatis.pojo
     mapper-locations: classpath:mybatis/mapper/*.xml
   ```

3. mybatis配置

4. ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
   PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
   "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.springbootdatamybatis.mapper.UserMapper">
   
   </mapper>
   ```

5. 编写Sql

   ```xml
   <select id="queryUserList" resultType="User">
     select * from study.User;
   </select>
   
   <select id="queryUserById" resultType="User">
     select * from User where #{id}
   </select>
   
   <insert id="addUser" parameterType="User">
     insert into User(id,username,userPassword) values(#{id},#{username},#{userPassword})
   </insert>
   
   <update id="updateUser" parameterType="User">
     update User set username=#{username},userPassword=#{userPassword} where id=#{id}
   </update>
   
   <delete id="deleteUser" parameterType="int">
     delete from User where id=#{id}
   </delete>
   ```

6. 业务层调用dao层

7. controller调用service层(这里直接用controller调用的dao层)

   ```java
   @RestController
   public class UserController {
       @Autowired
       private UserMapper userMapper;
       @GetMapping("/queryUserList")
       public List<User> queryUserList(){
           List<User> userList = userMapper.queryUserList();
           for (User user : userList) {
               System.out.println(user);
           }
           return userList;
       }
   }
   ```

# 15、SpringSecurity（安全）

## 安全简介

在 Web 开发中，安全一直是非常重要的一个方面。安全虽然属于应用的非功能性需求，但是应该在应用开发的初期就考虑进来。如果在应用开发的后期才考虑安全的问题，就可能陷入一个两难的境地：一方面，应用存在严重的安全漏洞，无法满足用户的要求，并可能造成用户的隐私数据被攻击者窃取；另一方面，应用的基本架构已经确定，要修复安全漏洞，可能需要对系统的架构做出比较重大的调整，因而需要更多的开发时间，影响应用的发布进程。因此，从应用开发的第一天就应该把安全相关的因素考虑进来，并在整个应用的开发过程中。

市面上存在比较有名的：Shiro，Spring Security ！

认证，授权

## 认识SpringSecurity

Spring Security 是针对Spring项目的安全框架，也是Spring Boot底层安全模块默认的技术选型，他可以实现强大的Web安全控制，对于安全控制，我们仅需要引入 spring-boot-starter-security 模块，进行少量的配置，即可实现强大的安全管理！

记住几个类：

- WebSecurityConfigurerAdapter：自定义Security策略
- AuthenticationManagerBuilder：自定义认证策略
- @EnableWebSecurity：开启WebSecurity模式

1. 引入 Spring Security 模块

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

1. 编写基础配置类

```java
package com.kuang.config;

@EnableWebSecurity // 开启WebSecurity模式
public class SecurityConfig extends WebSecurityConfigurerAdapter {
  @Override
  protected void configure(HttpSecurity http) throws Exception {

  }
}
```

1. 定制请求的授权规则

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
   // 定制请求的授权规则
   // 首页所有人可以访问
   http.authorizeRequests().antMatchers("/").permitAll()
  .antMatchers("/level1/**").hasRole("vip1")
  .antMatchers("/level2/**").hasRole("vip2")
  .antMatchers("/level3/**").hasRole("vip3");
}
```

1. 测试一下：发现除了首页都进不去了！因为我们目前没有登录的角色，因为请求需要登录的角色拥有对应的权限才可以！
2. 在configure()方法中加入以下配置，开启自动配置的登录功能！

```java
// 开启自动配置的登录功能
// "/login" 请求来到登录页
// /login?error 重定向到这里表示登录失败
http.formLogin();
```

 此时如果没有权限就会跳转至登陆页

1. 定义认定规则，重写configure(AuthenticationManagerBuilder auth)方法

```java
//认证
@Override
protected void configure(AuthenticationManagerBuilder auth) throws Exception {
  //数据正常应该从数据库中取，现在从内存中取
  auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
    .withUser("admin").password(new BCryptPasswordEncoder().encode("123")).roles("vip1","vip2")
    .and()
    .withUser("root").password(new BCryptPasswordEncoder().encode("123")).roles("vip1","vip2","vip3")
    .and()
    .withUser("guest").password(new BCryptPasswordEncoder().encode("123")).roles("vip1");
```

 这里设置密码加密`auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())`否则会报`There is no PasswordEncoder mapped for the id "null"`错误,创建的每个用户也必须添加密码加密**`.password(new BCryptPasswordEncoder().encode("123"))`**

1. 测试,发现，登录成功，并且每个角色只能访问自己认证下的规则！搞定

## 权限控制和注销

### 注销

1. 开启自动配置的注销的功能

   ```html
   //定制请求的授权规则
   @Override
   protected void configure(HttpSecurity http) throws Exception {
      //....
      //开启自动配置的注销的功能
         // /logout 注销请求
      http.logout();
   }
   ```

2. 我们在前端，增加一个注销的按钮，index.html 导航栏中

   ```html
   <a class="btn btn-primary" th:href="@{/logout}">注销</a>
   ```

3. 测试发现，点击注销按钮后会跳转到登陆页面，此时想要在注销成功后跳转到指定页面需要在请求后添加`.logoutSuccessUrl("/");`

   ```java
   // .logoutSuccessUrl("/"); 注销成功来到首页
   http.logout().logoutSuccessUrl("/");
   ```

### 权限控制

**需求：**用户没有登录的时候，导航栏上只显示登录按钮，用户登录之后，导航栏可以显示登录的用户信息及注销按钮！还有就是，比如admin这个用户，它只有 vip2，vip3功能，那么登录则只显示这两个功能，而vip1的功能菜单不显示！

1. 导入maven依赖

   ```xml
   <!-- https://mvnrepository.com/artifact/org.thymeleaf.extras/thymeleaf-extras-springsecurity4 -->
   <dependency>
      <groupId>org.thymeleaf.extras</groupId>
      <artifactId>thymeleaf-extras-springsecurity5</artifactId>
      <version>3.0.4.RELEASE</version>
   </dependency>
   ```

2. 导入命名空间

   ```html
   xmlns:sec="http://www.thymeleaf.org/extras/spring-security"
   ```

3. 修改index页面,增加判断

   ```html
   <div>
     <!--如果未登录则显示以下内容-->
     <div sec:authorize="!isAuthenticated()">
       <a class="btn btn-primary" th:href="@{/toLogin}">登陆</a>
     </div>
     <!--如果已登录则显示以下内容-->
     <div sec:authorize="isAuthenticated()">
       用户名<p sec:authentication="name"></p>
       角色<p sec:authentication="principal.authorities"></p>
       <a class="btn btn-primary" th:href="@{/logout}">注销</a>
     </div>
   </div>
   <!--如果用户拥有这个角色，则显示该div内的内容，如果没有则不显示-->
   <div class="div" sec:authorize="hasRole('vip1')">
     <a th:href="@{/level1/1}">level1-1</a><br/>
     <a th:href="@{/level1/2}">level1-2</a>
   </div>
   <div class="div" sec:authorize="hasRole('vip2')">
     <a th:href="@{/level2/1}">level2-1</a><br/>
     <a th:href="@{/level2/2}">level2-2</a>
   </div>
   <div class="div" sec:authorize="hasRole('vip3')">
     <a th:href="@{/level3/1}">level3-1</a><br/>
     <a th:href="@{/level3/2}">level3-2</a>
   </div>
   ```

4. 如果注销404了，就是因为它默认防止csrf跨站请求伪造，因为会产生安全问题，我们可以将请求改为post表单提交，或者在spring security中关闭csrf功能；我们试试：在 配置中增加 `http.csrf().disable();`

## 记住我

1. 开启记住我功能

   ```java
   //定制请求的授权规则
   @Override
   protected void configure(HttpSecurity http) throws Exception {
   //。。。。。。。。。。。
      //记住我,保存2周
      http.rememberMe();
   }
   ```

2. 测试，关闭浏览器再次打开用户依旧存在

   **本质上是保存到cookie，通过浏览器审查元素的application中可以看到**

3. 如果使用自己的页面中的按钮，可以给按钮设置name，再在配置后面加上如下方法

   ```java
   http.rememberMe().rememberMeParameter("remember");
   ```

## 定制登录页

1. 在配置中设置，用`.loginProcessingUrl("/login")`并将前端`form`表单的`action`设置为括号内相同即可，但如果前端用户名和密码前后端不一样，则需要进行设置，括号内的属性为前端页面的`name`属性

   ```java
   http.formLogin().loginPage("/toLogin").usernameParameter("username").passwordParameter("password").loginProcessingUrl("/login");
   ```

# 16、Shiro

## shiro简介

### 基本功能点

Shiro 可以非常容易的开发出足够好的应用，其不仅可以用在 JavaSE 环境，也可以用在 JavaEE 环境。Shiro 可以帮助我们完成：认证、授权、加密、会话管理、与 Web 集成、缓存等。其基本功能点如下图所示：
![img](https://gitee.com/wowoa/typoraPic/raw/master/image2020/20201229144118.jpg)

- Authentication：身份认证 / 登录，验证用户是不是拥有相应的身份；
- Authorization：授权，即权限验证，验证某个已认证的用户是否拥有某个权限；即判断用户是否能做事情，常见的如：验证某个用户是否拥有某个角色。或者细粒度的验证某个用户对某个资源是否具有某个权限；
- Session Manager：会话管理，即用户登录后就是一次会话，在没有退出之前，它的所有信息都在会话中；会话可以是普通 JavaSE 环境的，也可以是如 Web 环境的；
- Cryptography：加密，保护数据的安全性，如密码加密存储到数据库，而不是明文存储；
- Web Support：Web 支持，可以非常容易的集成到 Web 环境；
- Caching：缓存，比如用户登录后，其用户信息、拥有的角色 / 权限不必每次去查，这样可以提高效率；
- Concurrency：shiro 支持多线程应用的并发验证，即如在一个线程中开启另一个线程，能把权限自动传播过去；
- Testing：提供测试支持；
- Run As：允许一个用户假装为另一个用户（如果他们允许）的身份进行访问；
- Remember Me：记住我，这个是非常常见的功能，即一次登录后，下次再来的话不用登录了。

记住一点，Shiro 不会去维护用户、维护权限；这些需要我们自己去设计 / 提供；然后通过相应的接口注入给 Shiro 即可。

## Shiro的架构

### 外部

我们从外部来看 Shiro ，即从应用程序角度的来观察如何使用 Shiro 完成工作。如下图：

![img](https://gitee.com/wowoa/typoraPic/raw/master/image2020/20201229144119.jpg)

可以看到：应用代码直接交互的对象是 Subject，也就是说 Shiro 的对外 API 核心就是 Subject；其每个 API 的含义：

**Subject**：主体，代表了当前 “用户”，这个用户不一定是一个具体的人，与当前应用交互的任何东西都Subject，如网络爬虫，机器人等；即一个抽象概念；所有 Subject 都绑定到 SecurityManager，与 Subject 的所有交互都会委托给 SecurityManager；可以把 Subject 认为是一个门面；SecurityManager 才是实际的执行者；

**SecurityManager**：安全管理器；即所有与安全有关的操作都会与 SecurityManager 交互；且它管理着所有 Subject；可以看出它是 Shiro 的核心，它负责与后边介绍的其他组件进行交互，如果学习过 SpringMVC，你可以把它看成 DispatcherServlet 前端控制器；

**Realm**：域，Shiro 从 Realm 获取安全数据（如用户、角色、权限），就是说 SecurityManager 要验证用户身份，那么它需要从 Realm 获取相应的用户进行比较以确定用户身份是否合法；也需要从 Realm 得到用户相应的角色 / 权限进行验证用户是否能进行操作；可以把 Realm 看成 DataSource，即安全数据源。

也就是说对于我们而言，最简单的一个 Shiro 应用：

1. 应用代码通过 Subject 来进行认证和授权，而 Subject 又委托给 SecurityManager；
2. 我们需要给 Shiro 的 SecurityManager 注入 Realm，从而让 SecurityManager 能得到合法的用户及其权限进行判断。

**从以上也可以看出，Shiro 不提供维护用户 / 权限，而是通过 Realm 让开发人员自己注入。**

### 内部

接下来我们来从 Shiro 内部来看下 Shiro 的架构，如下图所示：

![img](https://gitee.com/wowoa/typoraPic/raw/master/image2020/20201229144120.jpg)

**Subject**：主体，可以看到主体可以是任何可以与应用交互的 “用户”；

**SecurityManager**：相当于 SpringMVC 中的 DispatcherServlet 或者 Struts2 中的 FilterDispatcher；是 Shiro 的心脏；所有具体的交互都通过 SecurityManager 进行控制；它管理着所有 Subject、且负责进行认证和授权、及会话、缓存的管理。

**Authenticator**：认证器，负责主体认证的，这是一个扩展点，如果用户觉得 Shiro 默认的不好，可以自定义实现；其需要认证策略（Authentication Strategy），即什么情况下算用户认证通过了；

**Authrizer**：授权器，或者访问控制器，用来决定主体是否有权限进行相应的操作；即控制着用户能访问应用中的哪些功能；

**Realm**：可以有 1 个或多个 Realm，可以认为是安全实体数据源，即用于获取安全实体的；可以是 JDBC 实现，也可以是 LDAP 实现，或者内存实现等等；由用户提供；注意：Shiro 不知道你的用户 / 权限存储在哪及以何种格式存储；所以我们一般在应用中都需要实现自己的 Realm；

**SessionManager**：如果写过 Servlet 就应该知道 Session 的概念，Session 呢需要有人去管理它的生命周期，这个组件就是 SessionManager；而 Shiro 并不仅仅可以用在 Web 环境，也可以用在如普通的 JavaSE 环境、EJB 等环境；所有呢，Shiro 就抽象了一个自己的 Session 来管理主体与应用之间交互的数据；这样的话，比如我们在 Web 环境用，刚开始是一台 Web 服务器；接着又上了台 EJB 服务器；这时想把两台服务器的会话数据放到一个地方，这个时候就可以实现自己的分布式会话（如把数据放到 Memcached 服务器）；

**SessionDAO**：DAO 大家都用过，数据访问对象，用于会话的 CRUD，比如我们想把 Session 保存到数据库，那么可以实现自己的 SessionDAO，通过如 JDBC 写到数据库；比如想把 Session 放到 Memcached 中，可以实现自己的 Memcached SessionDAO；另外 SessionDAO 中可以使用 Cache 进行缓存，以提高性能；

**CacheManager**：缓存控制器，来管理如用户、角色、权限等的缓存的；因为这些数据基本上很少去改变，放到缓存中后可以提高访问的性能

**Cryptography**：密码模块，Shiro 提高了一些常见的加密组件用于如密码加密 / 解密的。

## shiro组件

### 身份验证

**身份验证**，即在应用中谁能证明他就是他本人。一般提供如他们的身份 ID 一些标识信息来表明他就是他本人，如提供身份证，用户名 / 密码来证明。

在 shiro 中，用户需要提供 principals （身份）和 credentials（证明）给 shiro，从而应用能验证用户身份：

**principals**：身份，即主体的标识属性，可以是任何东西，如用户名、邮箱等，唯一即可。一个主体可以有多个 principals，但只有一个 Primary principals，一般是用户名 / 密码 / 手机号。

**credentials**：证明 / 凭证，即只有主体知道的安全值，如密码 / 数字证书等。

最常见的 principals 和 credentials 组合就是用户名 / 密码了。接下来先进行一个基本的身份认证。

另外两个相关的概念是之前提到的 **Subject** 及 **Realm**，分别是主体及验证主体的数据源。

## 快速开始（helloworld）

1. 导入依赖

   ```xml
   <dependency>
     <groupId>org.apache.shiro</groupId>
     <artifactId>shiro-core</artifactId>
     <version>1.4.1</version>
   </dependency>
   <dependency>
     <groupId>org.slf4j</groupId>
     <artifactId>slf4j-log4j12</artifactId>
     <version>1.7.21</version>
   </dependency>
   <dependency>
     <groupId>org.slf4j</groupId>
     <artifactId>jcl-over-slf4j</artifactId>
     <version>1.7.21</version>
   </dependency>
   <dependency>
     <groupId>log4j</groupId>
     <artifactId>log4j</artifactId>
     <version>1.2.17</version>
   </dependency>
   <dependency>
     <groupId>commons-logging</groupId>
     <artifactId>commons-logging</artifactId>
     <version>1.1.1</version>
   </dependency>
   ```

2. 配置文件（shiro.ini）

   ```ini
   [users]
   # user 'root' with password 'secret' and the 'admin' role
   root = secret, admin
   # user 'guest' with the password 'guest' and the 'guest' role
   guest = guest, guest
   # user 'presidentskroob' with password '12345' ("That's the same combination on
   # my luggage!!!" ;)), and role 'president'
   presidentskroob = 12345, president
   # user 'darkhelmet' with password 'ludicrousspeed' and roles 'darklord' and 'schwartz'
   darkhelmet = ludicrousspeed, darklord, schwartz
   # user 'lonestarr' with password 'vespa' and roles 'goodguy' and 'schwartz'
   lonestarr = vespa, goodguy, schwartz
   
   [roles]
   # 'admin' role has all permissions, indicated by the wildcard '*'
   admin = *
   # The 'schwartz' role can do anything (*) with any lightsaber:
   schwartz = lightsaber:*
   # The 'goodguy' role is allowed to 'drive' (action) the winnebago (type) with
   # license plate 'eagle5' (instance specific id)
   goodguy = winnebago:drive:eagle5
   ```

3. log4j

   ```properties
   log4j.rootLogger=INFO, stdout
   
   log4j.appender.stdout=org.apache.log4j.ConsoleAppender
   log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
   log4j.appender.stdout.layout.ConversionPattern=%d %p [%c] - %m %n
   
   # General Apache libraries
   log4j.logger.org.apache=WARN
   
   # Spring
   log4j.logger.org.springframework=WARN
   
   # Default Shiro logging
   log4j.logger.org.apache.shiro=INFO
   
   # Disable verbose logging
   log4j.logger.org.apache.shiro.util.ThreadContext=WARN
   log4j.logger.org.apache.shiro.cache.ehcache.EhCache=WARN
   ```

4. Quickstart.java

   ```java
   import org.apache.shiro.SecurityUtils;
   import org.apache.shiro.authc.*;
   import org.apache.shiro.config.IniSecurityManagerFactory;
   import org.apache.shiro.mgt.SecurityManager;
   import org.apache.shiro.session.Session;
   import org.apache.shiro.subject.Subject;
   import org.apache.shiro.util.Factory;
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   
   /**
    * @author lee
    * @date 2020/8/11 - 6:24 下午
    */
   
   public class Quickstart {
   
   
     private static final transient Logger log = LoggerFactory.getLogger(Quickstart.class);
   
     public static void main(String[] args) {
   
   
       Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro.ini");
       SecurityManager securityManager = factory.getInstance();
   
       SecurityUtils.setSecurityManager(securityManager);
   
       //获取当前的用户对象 Subject
       Subject currentUser = SecurityUtils.getSubject();
   
       // 通过当前用户拿到Session
       Session session = currentUser.getSession();
       session.setAttribute("someKey", "aValue");
       String value = (String) session.getAttribute("someKey");
       if (value.equals("aValue")) {
         log.info("Subject=>session[" + value + "]");
       }
   
       //判断当前用户是否被认证
       if (!currentUser.isAuthenticated()) {
         //Token： 令牌
         UsernamePasswordToken token = new UsernamePasswordToken("lonestarr", "vespa");
         token.setRememberMe(true);//设置记住我
         try {
           currentUser.login(token);//执行登陆操作
         } catch (UnknownAccountException uae) {
           log.info("There is no user with username of " + token.getPrincipal());
         } catch (IncorrectCredentialsException ice) {
           log.info("Password for account " + token.getPrincipal() + " was incorrect!");
         } catch (LockedAccountException lae) {
           log.info("The account for username " + token.getPrincipal() + " is locked.  " +
                    "Please contact your administrator to unlock it.");
         }
         // ... catch more exceptions here (maybe custom ones specific to your application?
         catch (AuthenticationException ae) {
           //unexpected condition?  error?
         }
       }
   
       //say who they are:
       //print their identifying principal (in this case, a username):
       log.info("User [" + currentUser.getPrincipal() + "] logged in successfully.");
   
       //test a role:
       if (currentUser.hasRole("schwartz")) {
         log.info("May the Schwartz be with you!");
       } else {
         log.info("Hello, mere mortal.");
       }
   
       //test a typed permission (not instance-level)
       if (currentUser.isPermitted("lightsaber:wield")) {
         log.info("You may use a lightsaber ring.  Use it wisely.");
       } else {
         log.info("Sorry, lightsaber rings are for schwartz masters only.");
       }
   
       //a (very powerful) Instance Level permission:
       if (currentUser.isPermitted("winnebago:drive:eagle5")) {
         log.info("You are permitted to 'drive' the winnebago with license plate (id) 'eagle5'.  " +
                  "Here are the keys - have fun!");
       } else {
         log.info("Sorry, you aren't allowed to drive the 'eagle5' winnebago!");
       }
   
       //all done - log out!
       currentUser.logout();
   
       System.exit(0);
     }
   }
   ```

以下方法Spring Security都有

```java
Subject currentUser = SecurityUtils.getSubject();
Session session = currentUser.getSession();
currentUser.isAuthenticated()
currentUser.getPrincipal()
currentUser.hasRole("schwartz")
currentUser.isPermitted("lightsaber:wield")
currentUser.logout();
```

## 在SpringBoot中集成Shiro

1. 创建springboot项目，创建时选择导入`springboot web`和`thmyeleaf`

2. 编写UserRealm类

   ```java
   //自定义的UserRealm
   public class UserRealm extends AuthorizingRealm {
     //授权
     @Override
     protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
       System.out.println("执行了=》授权");
       return null;
     }
     //认证
     @Override
     protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
       System.out.println("执行了=》认证");
       return null;
     }
   }
   ```

3. 编写Shiroconfig类(三个方法，从下往上写)

   ```java
   @Configuration
   public class ShiroConfig {
     //shiroFilterFactoryBean：3
     public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("securityManager") DefaultWebSecurityManager defaultWebSecurityManager){
       ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
       //设置安全管理器
       bean.setSecurityManager(defaultWebSecurityManager);
       return bean;
     }
     //DefaultWebSecurityManager：2
     @Bean(name="securityManager")
     public DefaultWebSecurityManager getdefaultWebSecurityManager(@Qualifier("userRealm") UserRealm userRealm){
       DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
       securityManager.setRealm(userRealm);
       return securityManager;
     }
   
     //创建realm对象：1
     @Bean
     public UserRealm userRealm(){
       return new UserRealm();
     }
   }
   ```

## 实现登陆拦截

1. 配置ShiroConfig配置类

   ```java
   //添加shiro的内置过滤器
           /*
               anon:无需认证
               authc:必须认证了才能访问
               user:必须拥有记住我功能才能用
               perms:拥有对某个资源的权限才能访问
               role:拥有某个角色权限才能访问
            */
           Map<String, String> filterMap = new LinkedHashMap<>();
           filterMap.put("/add","authc");
           filterMap.put("/update","authc");
        bean.setFilterChainDefinitionMap(filterMap);
   
           //如果没有权限就调到登陆页面
           bean.setLoginUrl("/toLogin");
   ```

2. 在controller类实现`/toLogin`请求

   ```java
   @RequestMapping("/toLogin")
   public String toLogin(){
   
     return "login";
   }
   ```

## 实现用户认证

1. 在controller配置请求

   ```java
   @RequestMapping("/login")
   public String login(String username ,String password,Model model){
   
     //获取当前的用户
     Subject subject = SecurityUtils.getSubject();
   
     //封装用户的登陆数据
     UsernamePasswordToken token = new UsernamePasswordToken(username,password);
   
     try {
       subject.login(token);//执行登陆方法，如果没有异常就说明登陆成功
       return "index";
     }catch (UnknownAccountException e){//用户名不存在
       model.addAttribute("msg","用户名不存在");
       return "login";
     }catch (IncorrectCredentialsException e){
       model.addAttribute("msg","密码错误");
       return "login";
     }
   
   }
   ```

2. 在UserRealm类中配置认证

   ```java
   //认证
   @Override
   protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
     System.out.println("执行了=》认证");
   
     //用户名密码   数据库取
     String name = "root";
     String password = "123";
   
     UsernamePasswordToken userToken = (UsernamePasswordToken) token;
     System.out.println(userToken.toString());
     if(!userToken.getUsername().equals(name)){
       return null; //抛出异常  UnknownAccountException
     }
   
     return new SimpleAuthenticationInfo("",password,"");
   }
   ```

   用户名验证自己来做，密码验证shiro来做

## 整合Mybatis

1. 配置properties(yaml),和日志配置

   ```java
   mybatis.type-aliases-package=com.shirospringboot.pojo
   mybatis.mapper-locations=classpath:mapper/*.xml
   ```

   ```yaml
   spring:
     datasource:
       username: codekitty
       password: lihaiyang
       url: jdbc:mysql://codekitty.cn:3306/study?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
       driver-class-name: com.mysql.cj.jdbc.Driver
       type: com.alibaba.druid.pool.DruidDataSource # 自定义数据源
   
       #Spring Boot 默认是不注入这些属性值的，需要自己绑定
       #druid 数据源专有配置
       initialSize: 5
       minIdle: 5
       maxActive: 20
       maxWait: 60000
       timeBetweenEvictionRunsMillis: 60000
       minEvictableIdleTimeMillis: 300000
       validationQuery: SELECT 1 FROM DUAL
       testWhileIdle: true
       testOnBorrow: false
       testOnReturn: false
       poolPreparedStatements: true
   
       #配置监控统计拦截的filters，stat:监控统计、log4j：日志记录、wall：防御sql注入
       #如果允许时报错  java.lang.ClassNotFoundException: org.apache.log4j.Priority
       #则导入 log4j 依赖即可，Maven 地址：https://mvnrepository.com/artifact/log4j/log4j
       filters: stat,wall,log4j
       maxPoolPreparedStatementPerConnectionSize: 20
       useGlobalDataSourceStat: true
       connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
   ```

2. 编写mapper接口和mapper.xml

   ```java
   @Repository
   @Mapper
   public interface UserMapper {
   
       public User queryUserByName(String name);
   }
   ```

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <!--namespace=绑定一个对应的Dao/Mapper接口-->
   <mapper namespace="com.shirospringboot.mapper.UserMapper">
       <!--    查询语句 id相当与要重写的方法名-->
       <select id="queryUserByName" resultType="User" parameterType="String">
           select * from study.User where username = #{username}
       </select>
   
   </mapper>
   ```

3. 写service层

   ```java
   public interface UserService {
       public User queryUserByName(String name);
   }
   ```

   ```java
   @Service
   public class UserServiceImpl implements UserService {
     @Autowired
     UserMapper userMapper;
   
     @Override
     public User queryUserByName(String username) {
   
       return userMapper.queryUserByName(username);
     }
   }
   ```

   测试项目可以直接调用dao，不写service层，但公司项目一般要求写service层

## 实现授权

1. 对资源设置权限

   ```java
   //拦截
   Map<String, String> filterMap = new LinkedHashMap<>();
   filterMap.put("/add","perms[user:add]");
   filterMap.put("/update","perms[user:update]");
   bean.setFilterChainDefinitionMap(filterMap);
   
   //未授权就跳转到此请求
   bean.setUnauthorizedUrl("/noauth");
   ```

2. 对登陆用户进行授权,该用户拥有的权限从数据库取得

   ```java
   //授权
   @Override
   protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
     System.out.println("执行了=》授权");
   
     SimpleAuthorizationInfo info = new SimpleAuthorizationInfo();
   
     //拿到当前登陆的对象
     Subject subject = SecurityUtils.getSubject();
     User currentUser = (User)subject.getPrincipal();//拿到User
     info.addStringPermission(currentUser.getPerms());//授权
   
     return info;
   }
   ```

## 整合thymeleaf

1. 导入shiro整合thymeleaf的maven资源

   ```xml
   <!--        shiro-thymeleaf整合-->
   <dependency>
     <groupId>com.github.theborakompanioni</groupId>
     <artifactId>thymeleaf-extras-shiro</artifactId>
     <version>2.0.0</version>
   </dependency>
   ```

2. 导入命名空间（不导没有提示，但不影响使用）

   ```html
   xmlns:shiro="http://www.thymeleaf.org/thymeleaf-extras-shiro"
   ```

3. 配置bean

   ```java
   @Bean
   //整合shiroDialect ：用来整合shiro和thymeleaf
   public ShiroDialect getShiroDialect(){
   
     return new ShiroDialect();
   }
   ```

4. 实现登陆后只显示有用户权限的页面的链接

   ```html
   <div shiro:hasPermission="user:add">
     <a th:href="@{/add}">add</a>
   </div>
   <div shiro:hasPermission="user:update">
     <a th:href="@{/update}">update</a>
   </div>
   ```

# 17、Swagger

接口文档对于前后端开发人员都十分重要。尤其近几年流行前后端分离后接口文档又变成重中之重。接口文档固然重要，但是由于项 目周期等原因后端人员经常出现无法及时更新，导致前端人员抱怨接 口文档和实际情况不一致。

很多人员会抱怨别人写的接口文档不规范，不及时更新。当时当 自己写的时候确实最烦去写接口文档。这种痛苦只有亲身经历才会牢 记于心。

如果接口文档可以实时动态生成就不会出现上面问题。

**Swagger** 可以完美的解决上面的问题。

## Swagger 简介：

Swagger 是一套围绕 Open API 规范构建的开源工具，可以帮助设 计，构建，记录和使用 REST API。

Swagger 工具包括的组件：

- Swagger Editor ：基于浏览器编辑器，可以在里面编写 Open API规范。类似 Markdown 具有实时预览描述文件的功能。
- Swagger UI：将 Open API 规范呈现为交互式 API 文档。用可视化UI 展示描述文件。
- Swagger Codegen：将 OpenAPI 规范生成为服务器存根和客户端 库。通过 Swagger Codegen 可以将描述文件生成 html 格式和 cwiki 形 式的接口文档，同时也可以生成多种言语的客户端和服务端代码。

Swagger Inspector：和 Swagger UI 有点类似，但是可以返回更多 信息，也会保存请求的实际参数数据。

Swagger Hub：集成了上面所有项目的各个功能，你可以以项目 和版本为单位，将你的描述文件上传到 Swagger Hub 中。在 Swagger Hub 中可以完成上面项目的所有工作，需要注册账号，分免费版和收费版。

使用 Swagger，就是把相关的信息存储在它定义的描述文件里面（yml 或 json 格式），再通过维护这个描述文件可以去更新接口文档， 以及生成各端代码。

## Swagger集成

1. 导入maven依赖

   ```xml
   <!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
   <dependency>
     <groupId>io.springfox</groupId>
     <artifactId>springfox-swagger-ui</artifactId>
     <version>2.9.2</version>
   </dependency>
   <!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
   <dependency>
     <groupId>io.springfox</groupId>
     <artifactId>springfox-swagger2</artifactId>
     <version>2.9.2</version>
   </dependency>
   ```

2. 创建SwaggerConfig.java

   ```java
   @Configuration
   @EnableSwagger2   //开启Swagger2
   public class SwaggerConfig{
   
   }
   ```

3. 运行访问`http://localhost:8080/swagger-ui.html`进行测试

## Swagger配置

在SwaggerConfig进行如下配置即可自定义Swagger配置

```java
@Configuration
@EnableSwagger2   //开启Swagger2
public class SwaggerConfig{
  @Bean
  public Docket docket(){

    return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo());
  }
  private ApiInfo apiInfo(){
    //作者信息
    Contact contact = new Contact("李海洋", "codekitty.cn:8000", "L18249290950@126.com");

    return new ApiInfo("codekitty's Swagger API Documentation", "千里之行，始于足下", "1.0", "codekitty.cn:8000",contact , "Apache 2.0", "http://www.apache.org/licenses/LICENSE-2.0", new ArrayList());
  }
}
```

## Swagger扫描接口

1. Docket(DocumentationType.SWAGGER_2).select()

```java
new Docket(DocumentationType.SWAGGER_2)
  .apiInfo(apiInfo())
  .enable(b)//是否启用Swagger，false则不启用
  .select()
  //RequestHandlerSelectors,配置要扫描接口的方式
  //basePackage，指定扫描包
  //                .apis(RequestHandlerSelectors.any()),扫描全部
  //                .apis(RequestHandlerSelectors.none()),不扫描
  //                withMethodAnnotation(GetMapping.class)) //扫描类上的注解
  //                withClassAnnotation(GetMapping.class)) //扫描方法上的注解
  .apis(RequestHandlerSelectors.basePackage("com.controller"))
  //Path过滤路径
  //                .paths(PathSelectors.ant("/com/**"))

  .build();
```

1. 实现开发环境中使用Swagger，运行上线环境中不使用Swagger

```java
public Docket docket(Environment environment){
//设置要显示的Swagger环境
Profiles profiles = Profiles.of("dev","test");
//获取项目的环境：判断是否处在自己设定的环境中
boolean flag = environment.acceptsProfiles(profiles);
}
```

在properties中选择使用的环境,将flag传入`Enable()`

1. 通过`groupName("")`配置多个Docket，(在多人开发中，每个开发者配置一个自己的Swagger，方便管理)

```java
@Bean
public Docket docket1(){
  return new Docket(DocumentationType.SWAGGER_2).groupName("A");
}
@Bean
public Docket docket2(){
  return new Docket(DocumentationType.SWAGGER_2).groupName("B");
}
@Bean
public Docket docket3(){
  return new Docket(DocumentationType.SWAGGER_2).groupName("C");
}
```

## 实体类配置

```java
@ApiModel("用户实体类")
public class User {
    @ApiModelProperty("用户名")
    private String username;
    @ApiModelProperty("密码")
    private String password;
```

在Controller进行配置请求

```java
@ApiOperation("user请求")
    //只要接口中返回值存在实体类，它就会被扫描到Swagger中
    @PostMapping(value = "/user")
    public String user(@ApiParam("用户名")String username){
        return "hello" + username;
    }
```

就可以在Swagger显示实体类的信息，如果属性是private需要加get,set方法

**总结：**

1. 我们可以通过Swagger给一些比较难理解的属性或接口增加注释信息
2. 接口文档实时更新
3. 可以在线测试

【注意】在正式发布时要关闭Swagger！！！可以保证安全和避免浪费性能

# 18、任务

## 异步任务

定义一个Service

```java
@Service
public class AsyncService {
    @Async
    public void hello(){
        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("数据正在处理");
    }
}
```

定义一个请求,在请求中调用service中的方法

```java
@ResponseBody
@RequestMapping("/hello")
public String hello(){
  asyncService.hello();
  return "OK";
}
```

在Service的方法中使用`@Async`,并在主入口上使用`@EnableAsync`开启异步任务

## 邮件发送

在properties中配置自己的邮件信息

```pro
spring.mail.username=1012074120@qq.com
spring.mail.password=jtnhlvabxxqsbbga
spring.mail.host=smtp.qq.com

#开启加密验证(qq邮箱)
spring.mail.properties.mail.smtp.ssl.enable=true
```

**简单邮件发送**

直接调用SpringBoot的`JavaMailSenderImpl`类,使用`SimpleMailMessage`发送简单邮件

```java
@Autowired
JavaMailSenderImpl mailSender;

@Test
void contextLoads() {
  SimpleMailMessage simpleMailMessage = new SimpleMailMessage();
  simpleMailMessage.setSubject("你好");
  simpleMailMessage.setText("Hello world");
  simpleMailMessage.setTo("L18249290950@126.com");
  simpleMailMessage.setFrom("1012074120@qq.com");
  mailSender.send(simpleMailMessage);
}
```

**复杂邮件发送**

调用`mailSender.createMimeMessage()`并使用`MimeMessageHelper`配置邮件内容，发送即可，邮件内容后设置为true可以解析html格式的内容

```java
public void SendMail(Boolean html,String title, String text, File file, String sendTo, String sendFrom) throws MessagingException {
        //复杂邮件
        MimeMessage mimeMessage = mailSender.createMimeMessage();
        //组装
        MimeMessageHelper mimeMessageHelper = new MimeMessageHelper(mimeMessage,true);

        mimeMessageHelper.setSubject(title);
        mimeMessageHelper.setText(text,true);//true,开启html解析
       mimeMessageHelper.addAttachment("1.jpg",file);

        mimeMessageHelper.setTo(sendTo);
        mimeMessageHelper.setFrom(sendFrom);
        mailSender.send(mimeMessage);
    }
```

## 定时任务

```java
TaskScheduler //任务调度程序
TaskExecutor	//任务执行者
@EnableScheduling //开启定时功能的注解，放在主入口
@Scheduled		//什么时候执行
  
cron表达式
```

在方法上面加上`@Scheduled(cron = "0 43 14 * * ?")`并加上相应的表达式即可

常用表达式例子：

```
（1）0 0 2 1 * ? *   表示在每月的1日的凌晨2点调整任务
（2）0 15 10 ? * MON-FRI   表示周一到周五每天上午10:15执行作
（3）0 15 10 ? 6L 2002-2006   表示2002-2006年的每个月的最后一个星期五上午10:15执行作
（4）0 0 10,14,16 * * ?   每天上午10点，下午2点，4点
（5）0 0/30 9-17 * * ?   朝九晚五工作时间内每半小时
（6）0 0 12 ? * WED    表示每个星期三中午12点
（7）0 0 12 * * ?   每天中午12点触发
（8）0 15 10 ? * *    每天上午10:15触发
（9）0 15 10 * * ?     每天上午10:15触发
（10）0 15 10 * * ? *    每天上午10:15触发
（11）0 15 10 * * ? 2005    2005年的每天上午10:15触发
（12）0 * 14 * * ?     在每天下午2点到下午2:59期间的每1分钟触发
（13）0 0/5 14 * * ?    在每天下午2点到下午2:55期间的每5分钟触发
（14）0 0/5 14,18 * * ?     在每天下午2点到2:55期间和下午6点到6:55期间的每5分钟触发
（15）0 0-5 14 * * ?    在每天下午2点到下午2:05期间的每1分钟触发
（16）0 10,44 14 ? 3 WED    每年三月的星期三的下午2:10和2:44触发
（17）0 15 10 ? * MON-FRI    周一至周五的上午10:15触发
（18）0 15 10 15 * ?    每月15日上午10:15触发
（19）0 15 10 L * ?    每月最后一日的上午10:15触发
（20）0 15 10 ? * 6L    每月的最后一个星期五上午10:15触发
（21）0 15 10 ? * 6L 2002-2005   2002年至2005年的每月的最后一个星期五上午10:15触发
（22）0 15 10 ? * 6#3   每月的第三个星期五上午10:15触发
```

# 19、集成Redis

Springboot操作数据：spring-data jpa jdbc mongodb redis

SpringData 也是和SpringBoot齐名的项目

说明：在SpringBoot2.x之后，原来使用的jedis被替换为乐lettuce

jedis：底层采用直连，多线程操作不安全，如果想要避免不安全，使用jedis pool连接池 更像Bio模式

lettuce：底层采用netty，实例可以在多个线程中共享，不存在线程不安全的情况，可以减少线程数量，更像Nio模式

## 集成

源码分析

```java
@Bean
@ConditionalOnMissingBean(
  name = {"redisTemplate"}//我们可以自己定义一个RedisTamplate来替换这个默认的
)
public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) throws UnknownHostException {
  //默认的RedisTemplate没有过多的设置，redis对象都是需要序列化的！
  //两个泛型都是<Object,Object>的类型，我们使用需要强制转换成<String,Object>
  RedisTemplate<Object, Object> template = new RedisTemplate();
  template.setConnectionFactory(redisConnectionFactory);
  return template;
}

@Bean
@ConditionalOnMissingBean
//由于String类型时redis中最长使用的类型，所以单独提取出来一个bean
public StringRedisTemplate stringRedisTemplate(RedisConnectionFactory redisConnectionFactory) throws UnknownHostException {
  StringRedisTemplate template = new StringRedisTemplate();
  template.setConnectionFactory(redisConnectionFactory);
  return template;
}
```

1. 导入依赖

   ```xml
   <!--        操作redis-->
   <dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-data-redis</artifactId>
   </dependency>
   ```

2. 配置连接

   ```properties
   #配置redis
   spring.redis.host=127.0.0.1
   spring.redis.port=6379
   ```

3. 测试

   ```java
   @Autowired
   private  RedisTemplate<Object,  Object>  redisTemplate;
   @Test
   void contextLoads() {
     //opsForValue   操作字符串 类似String
     //opsForList    操作List 类似List
     //opsForSet     操作
     //opsForHash
     //opsForZSet
     //opsForGeo
     //opsForHyperLogLog
   
     //除了基本的操作，我们常用的方法都可以直接通过redisTemplate操作，比如事务和基本的CRUD
     //        RedisConnection connection = redisTemplate.getConnectionFactory().getConnection();
     //        connection.flushDb();
     //        connection.flushAll();
   
     redisTemplate.opsForValue().set("mykey","李明");
     System.out.println((String)redisTemplate.opsForValue().get("mykey"));
   }
   ```

   输出“李明”既为测试成功

后续等学过redis后再记

# 分布式Dubbo+Zokeeper+SpringBoot

## **什么是分布式系统？**

在《分布式系统原理与范型》一书中有如下定义：“分布式系统是若干独立计算机的集合，这些计算机对于用户来说就像单个相关系统”；

分布式系统是由一组通过网络进行通信、为了完成共同的任务而协调工作的计算机节点组成的系统。分布式系统的出现是为了用廉价的、普通的机器完成单个计算机无法完成的计算、存储任务。其目的是**利用更多的机器，处理更多的数据**。

分布式系统（distributed system）是建立在网络之上的软件系统。

首先需要明确的是，只有当单个节点的处理能力无法满足日益增长的计算、存储任务的时候，且硬件的提升（加内存、加磁盘、使用更好的CPU）高昂到得不偿失的时候，应用程序也不能进一步优化的时候，我们才需要考虑分布式系统。因为，分布式系统要解决的问题本身就是和单机系统一样的，而由于分布式系统多节点、通过网络通信的拓扑结构，会引入很多单机系统没有的问题，为了解决这些问题又会引入更多的机制、协议，带来更多的问题。。。

## 什么是RPC

RPC【Remote Procedure Call】是指远程过程调用，是一种进程间通信方式，他是一种技术的思想，而不是规范。它允许程序调用另一个地址空间（通常是共享网络的另一台机器上）的过程或函数，而不用程序员显式编码这个远程调用的细节。即程序员无论是调用本地的还是远程的函数，本质上编写的调用代码基本相同。

也就是说两台服务器A，B，一个应用部署在A服务器上，想要调用B服务器上应用提供的函数/方法，由于不在一个内存空间，不能直接调用，需要通过网络来表达调用的语义和传达调用的数据。为什么要用RPC呢？就是无法在一个进程内，甚至一个计算机内通过本地调用的方式完成的需求，比如不同的系统间的通讯，甚至不同的组织间的通讯，由于计算能力需要横向扩展，需要在多台机器组成的集群上部署应用。RPC就是要像调用本地的函数一样去调远程函数；

Dubbo基本概念

![img](https://gitee.com/wowoa/typoraPic/raw/master/image2020/20201229144121.jpg)

**服务提供者**（Provider）：暴露服务的服务提供方，服务提供者在启动时，向注册中心注册自己提供的服务。

**服务消费者**（Consumer）：调用远程服务的服务消费方，服务消费者在启动时，向注册中心订阅自己所需的服务，服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。

**注册中心**（Registry）：注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者

**监控中心**（Monitor）：服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心

**调用关系说明**

l 服务容器负责启动，加载，运行服务提供者。

l 服务提供者在启动时，向注册中心注册自己提供的服务。

l 服务消费者在启动时，向注册中心订阅自己所需的服务。

l 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者。

l 服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。

l 服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心。

安装Dubbo-admin

1. 下载dubbo-admin

地址 ：https://github.com/apache/dubbo-admin/tree/master

1. 进行打包（可以是Idea打包，也可以是命令行打包）

2. 使用命令`java -jar dubbo-admin-0.0.1-SNAPSHOT.jar`

   【注意：zookeeper的服务一定要打开！】

3. 访问localhost:7001即可访问Dubbo-admin页面

   ![img](https://gitee.com/wowoa/typoraPic/raw/master/image2020/20201229144122.jpg)

### 实现跨项目访问类

1. 提供者配置文件

   ```properties
   #服务应用名字
   dubbo.application.name=provider-server
   #注册中心地址
   dubbo.registry.address=zookeeper://127.0.0.1:2181
   #哪些服务要被注册(扫描包)
   dubbo.scan.base-packages=com.servic
   ```

2. 编写提供者的接口和实现类

3. 消费者配置文件

   ```properties
   #消费者去哪里拿服务需要暴露自己的名字
   dubbo.application.name=customer-server
   
   #注册中心的地址
   dubbo.registry.address=zookeeper://127.0.0.1:2181
   ```

4. 在消费者和提供者相同的包下建立提供者的接口

5. 消费者服务类

   ```java
   @Service //注入到容器中
   public class UserService {
   
      @Reference //远程引用指定的服务，他会按照全类名进行匹配，看谁给注册中心注册了这个全类名
      TicketService ticketService;
   
      public void bugTicket(){
          String ticket = ticketService.getTicket();
          System.out.println("在注册中心买到"+ticket);
     }
   ```

6. 编写测试类

   ```java
   @Autowired
   UserService userService;
   
   @Test
   public void contextLoads() {
   
     userService.bugTicket();
   
   }
   ```

## **启动测试**

**1. 开启zookeeper**

**2. 打开dubbo-admin实现监控【可以不用做】**

**3. 开启服务者**

**4. 消费者消费测试**

# 回顾,架构!

## 层架构+MVC

架构 -->解耦

开发框架

 Spring

 IOC AOP

 **IOC : 控制反转**

 泡温泉,泡茶泡友

 附近的人,打招呼。加微信,聊天,天天聊>约泡

 浴场(容器):温泉,茶庄泡友

 直接进温泉,就有人和你一起了!

 原来我们都是自己一步步操作,现在交给容器了!我们需要什么就去拿就可以了
​ **AOP:切面(本质,动态代理）**
​ 为了解什么?不影响业本来的情况下,实现动态增加功能,大量应用在日志,事务等等

 Spring是一个轻量级的Java开源框架，容器

 目的：解决企业开发的复杂性问题

 Spring是春天，但配置文件繁琐

## SpringBoot

**SpringBoot** ,新代javaEE的开发标准,开箱即用!>拿过来就可以用,它自

动帮我们配置了非常多的东西,我们拿来即用,特性:约定大于配置!

随着公司体系越来越大,用户越来越多

微服务架构—>新架构

 模块化,功能化!

 用户,支付,签到,娱乐…;

 人多余多一台服务器解决不了就再增加一台服务器! --横向扩展

 假设A服务器占用98%资源B服务器只占用了10%.–**负载均衡**;

 将原来的整体项,分成模块化,用户就是一个单独的项目,签到也是一个单独的项目,项目和项目之前需要通信,如何通信

 用户非常多而到十分少给用户多一点服务器,给签到少一点服务器
微服务架构问题?
**分布式架构会遇到的四个核心题?**

1. 这么多服务,客户端该如何去访?
2. 这么多服务,服务之间如何进行通信?
3. 这么多服务,如何治理呢?

## **解决方案:SpringCloud**

Springcloud是一套生态，就是来解决以上分布式架构的4个问题

想使用Spring Clould ,必须要掌握 springBoot , 因为Springcloud是基于springBoot ;

1. spring Cloud NetFlix

    

   ,出来了一套解决方案！一站式解决方案。可以直接使用

   - Api网关 , zuul组件
   - Feign --> Httpclient —> http通信方式,同步并阻塞
   - 服务注册与发现 , Eureka
   - 熔断机制 , Hystrix

2018年年底,NetFlix 宣布无限期停止维护。生态不再维护,就会脱节。

1. **Apache Dubbo zookeeper ,**

   - API:没有!要么找第三方组件,要么自己实现
   - Dubbo 是一个高性能的基于ava实现的RPC通信框架!2.6.x
   - 服务注册与发现 , zookeeper :动物管理者 ( Hadoop , Hive )
   - 没有:借助了Hystrix

   不完善，Dubbo

2. **SpringCloud Alibaba** 一站式解决方案

目前又提出了新的思路

- 服务网格：也许是下一代维服务标准，Service mesh
- 代表解决方案：istio（未来可能需要掌握）



总而言之，要解决的问题就是4个

1. API网关 ， 服务路由
2. HTTP，RPC框架，异步调用
3. 服务注册与发现，高可用
4. 熔断机制，服务降级

为什么要解决这个问题？因为网络是不可靠的