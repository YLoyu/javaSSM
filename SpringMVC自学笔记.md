# 1、SpringMVC

MVC:model,view,Controller模型 视图 控制器

### SpringMVC的执行流程！【重点】

![image-20201022101503552](E:\Document\笔记\SSM框架\SSMmd笔记\img\image-20201022101503552.png)

### MVC:模型（dao）视图    控制器

### 什么是MVC

- mvc是一种软件设计规范
- mvc主要作用是降低了视图和业务逻辑间的双向耦合
- mvc是一种架构模式

SpringMVC的特点：

- 轻量级，简单易学
- 高效，基于请求响应的MVC框架
- 与Spring兼容性好，无缝结合
- 约定优于配置
- 功能强大：RESTful、数据验证、格式化、本地化、主题等
- 简答灵活

# 2、SpringMVC的入门案例

实现步骤：

1. 新建一个web项目
2. 导入相关jar包
3. 编写web.xml,注册DispatchServlet
4. 编写springmvc配置文件
5. 接下来就是去创建对应的控制类，controller
6. 最后完善前端视图和Controller之间的对应
7. 测试运行调试

注意：使用springmvc必须配置的三大件    处理器映射器、处理器适配器、视图解析器

通常，我们 **只需要手动配置视图解析器**，而 **处理器映射器** 和 **处理器适配器** 只需要开启注解驱动即可

他们属于弱耦合

### pom.xml（依赖和过滤器）

```xml
<!--导入依赖-->
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.0.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.1</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.12</version>
        </dependency>

        <!--导入json包-->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.9.7</version>
        </dependency>
        <!--导入fastjson-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.47</version>
        </dependency>
    </dependencies>

    <build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
        </resources>
    </build>
```



### web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">



<!--注册servlet-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--通过初始化参数指定springMVC配置文件的位置-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <!--启动顺序，数字越小，启动越早-->
        <load-on-startup>1</load-on-startup>
    </servlet>

    <!--所有请求都会被springmvc拦截-->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

	<!--配置SpringMVC的乱码过滤-->
    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    
    
</web-app>
```

### springmvc-servlet.xml 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">


    <context:component-scan base-package="com.bdqn.controller"/><!--自动扫描包，让指定包下的注解生效，由IOC容器统一管理-->
    <mvc:default-servlet-handler/><!--资源过滤器，专门过滤静态资源的数据-->
    <mvc:annotation-driven/><!--帮我们映射类和处理方法的注解  相当于处理器的映射器，和适配器-->

    <!--JSON乱码问题配置-->
    <mvc:annotation-driven>
        <mvc:message-converters register-defaults="true">
            <bean class="org.springframework.http.converter.StringHttpMessageConverter">
                <constructor-arg value="utf-8"/>
            </bean>
            <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
                <property name="objectMapper">
                    <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                        <property name="failOnEmptyBeans" value="false"/>
                    </bean>
                </property>
            </bean>
        </mvc:message-converters>
    </mvc:annotation-driven>

    
    
    <!--视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <!--前缀-->
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <!--后缀-->
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```

### controller类

- @Controller 
  - 代表这个类会被Spring托管，被这个注解的类，中的所有方法，
  - 如果返回值是String，并且有具体的页面可以跳转，那么就会被视图解析器解析
  - 方法的参数写的是前端接收的数据
- @RestController
  - 他下面所有的方法只会返回字符串
  - @ResponseBody 指定这个方法返回的是一个字符串
- @RequestMapping
  - 如果在类上写就相当于多一层目录
  - 如果是在方法上写就是指定跳转到哪个页面

```java
package com.bdqn.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
//@RequestMapping("/hello")// 多一层目录
public class HelloController {
    @RequestMapping("/hello")// 通过该注解来指定跳转到哪个页面      
    public String hello(Model model){// 通过model来实现封装数据
        //封装数据
        model.addAttribute("msg","hello,SpringMVCAnnotation!");
        return "hello"; // 返回的就是试图的名字，会被视图解析器处理
    }
}
```

### JsonUtils（转格式用）

```java
package com.bdqn.utils;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.SerializationFeature;

import java.text.SimpleDateFormat;

public class JsonUtils {

    public static String getJson(Object object){
        return getJson(object,"yyyy-MM-dd HH:mm:ss");
    }


    public static String getJson(Object object,String dataFormat){
        // 利用jackson下的ObjectMapper转化格式
        ObjectMapper mapper = new ObjectMapper();
        // 不使用时间戳的方式
        mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS,false);
        // 自定义日期格式
        SimpleDateFormat sdf = new SimpleDateFormat(dataFormat);
        mapper.setDateFormat(sdf);
        try {
            return mapper.writeValueAsString(object);
        } catch (JsonProcessingException e) {
            e.printStackTrace();
        }
        return null;
    }

}
```

# 3、ReslFull风格

### 概念

Result就是一个资源定位及操作的风格。不是标准也不是协议，只是一种风格。基于这个风格设计的软件可以更加简洁，更有层次，更易于实现缓存等级制。

### 功能

- 资源：互联网所有的事物都可以被抽象为资源
- 资源操作：使用POST、DELETE、PUT、GET，使用不同方法对资源进行操作。
- 分别对应 添加、删除、修改、查询

以前的风格：http://localhost:8080/add?a=1&b=2

RestFul: http://localhost:8080/add/a/b

### 测试

1、在SpringMVC中可以使用@PathVariable注解，让方法参数的值对应绑定到一个URI模板变量上。

```java
@Controller
public class RestFulController {
    @RequestMapping(value = "/add/{a}/{b}",method = RequestMethod.GET)
    public String test01(@PathVariable int a,@PathVariable int b, Model model){
        int res = a+b;
        model.addAttribute("msg","结果为"+res);
        return "test";
    }
}

```

2、使用Method属性指定请求类型

- GET
- POST
- HEAD
- OPTIONS
- PUT
- PATCH
- DELETE
- TRACE等等

@RequestMapping(value = "/add/{a}/{b}",method = RequestMethod.上面参数)

**小结：SpringMVC的@RequestMapper注解能够处理HTTP请求的方法，比如GET，PUT，POST、DELETE、以及PATCH**

**所有的地址栏请求默认都会是HTTP GET类型的**



3、也可以通过：组合注解

比如：@GetMapping("/add/{a}/{b}")

```java
@Controller
public class RestFulController {
//    @RequestMapping(value = "/add/{a}/{b}",method = RequestMethod.DELETE)
    @GetMapping("/add/{a}/{b}")
    public String test01(@PathVariable int a,@PathVariable int b, Model model){
        int res = a+b;
        model.addAttribute("msg","结果为"+res);
        return "test";
    }
}
```

- @GetMapping
- @PostMapping
- @PutMapping
- @DeleteMapping
- @PatchMapping

可以直接代替：@RequestMapping(value = "/add/{a}/{b}",method = RequestMethod.DELETE)

# 4、重定向和转发

### 1、在没有视图解析器的情况下

转发：

```java
@Controller
public class RestFulController {
//    @RequestMapping(value = "/add/{a}/{b}",method = RequestMethod.DELETE)
    @GetMapping("/add/{a}/{b}")
    public String test01(@PathVariable int a,@PathVariable int b, Model model){
        int res = a+b;
        model.addAttribute("msg","结果为"+res);
        // 转发
        return "forward:/WEB_INF/jsp/test.jsp";
    }
}
```

重定向：

```java
@Controller
public class RestFulController {
//    @RequestMapping(value = "/add/{a}/{b}",method = RequestMethod.DELETE)
    @GetMapping("/add/{a}/{b}")
    public String test01(@PathVariable int a,@PathVariable int b, Model model){
        int res = a+b;
        model.addAttribute("msg","结果为"+res);
        // 重定向
        return "redirect:/WEB_INF/jsp/test.jsp";
    }
}
```

小结：

- 转发的关键字是：forward
- 重定向的关键字是：response
- 转发的网址必须是全限定类名 



### 2、有视图解析器的情况下

```java
@Controller
public class RestFulController {
    @GetMapping("/add/{a}/{b}")
    public String test01(@PathVariable int a,@PathVariable int b, Model model){
        int res = a+b;
        model.addAttribute("msg","结果为"+res);
        return "test";// 转发
    }
    @GetMapping("/ad/{a}/{b}")
    public String test02(@PathVariable int a,@PathVariable int b, Model model){
        int res = a+b;
        model.addAttribute("msg","结果为"+res);
        return "redirect:test";// 转发
    }
}
```

小结：

- 默认是转发
- 加上redirect就是重定向

# 5、MVC数据处理

### 1、提交的域名称和处理方法的参数名一致

例如：

提交数据：http://localhost:8080/hello?name=测试

处理方法：

```java
@RequestMapping()
public String hello(String name){
	System.out.println(name);
	return "hello";
}
```

后台输出：测试

### 2、提交的域名称和处理方法的参数名不一致

例如：

提交数据：http://localhost:8080/hello?username=测试

处理方法：

```java
@RequestMapping()
public String hello(@RequestParam("username") String name){
	System.out.println(name);
	return "hello";
}
```

后台输出：测试

### 3、提交的是一个对象

步骤：

1. 需要一个实体类

2. 提交数据：http://localhost:8080/user/t2?id=1&name=3&age=2

3. 处理方法：

   ```java
   // 前端接收了一个对象 : id,name,age
       @GetMapping("/t2")
       public String test02(User user){
           System.out.println(user);
           return "test";
       }
   ```

   后台输出：User{id=1, name='3', age=2}



**注：如果使用对象的话，前端传递的域名必须和对象的参数名必须一致，否则就是null**

### 小结：

- @RequestParam("username") ： 更改参数的名称    如果不使用，那么前端传来的域名必须和参数名一致





# 6、数据显示到前端（三种方法）

### 1、第一种：通过ModelAndView

```java
public class controller implements Controller {
    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        ModelAndView mv = new ModelAndView();
        // 业务代码
        String res = "helloSpringMVC";
        mv.addObject("msg",res);
        // 视图跳转
        mv.setViewName("test");
        return mv;
    }
}
```

### 2、第二种：通过ModelMap

```java
  @GetMapping("/t1")
    public String test01(@RequestParam("username") String name, Model model){
        // 1.接收前端参数
        System.out.println("接收到前端的参数为："+name);
        // 2.将返回的结果传递给前端 , model
        model.addAttribute("msg",name);
        // 3.跳转视图
        return "test";
    }
```



### 3、第三种：通过Model（常用）

```java
    @GetMapping("/t1")
    public String test01(@RequestParam("username") String name, Model model){
        // 1.接收前端参数
        System.out.println("接收到前端的参数为："+name);
        // 2.将返回的结果传递给前端 , model
        model.addAttribute("msg",name);
        // 3.跳转视图
        return "test";
    }
```

### 区别

- Model : 只有几个方法适合用于存储数据，简化版（用的也最多）
- ModelMap：继承了LinkedMap，除了实现自身的一些方法，同时也继承了Linked的方法和特性！
- ModelAndView：可以在存储数据的同时，可以进行设置返回的逻辑视图，进行控制展示层的跳转

# 7、乱码问题

### 测试：

1、编写表单

```xml
<form action="e/t1" method="post">
    <input type="text" name="name">
    <input type="submit">
</form>
```

2、后台编写对应的处理类

```java
 @PostMapping("e/t1")
public String test01(String name,Model model){
    model.addAttribute("msg",name);
    return "test";
}
```

3、结果：为乱码

```
å²¸ä¸å¤§èµå¤§
```

### 解决：通过过滤器解决乱码

配置springmvc的乱码过滤

```xml
<!--配置SpringMVC的乱码过滤-->
    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```



# 8、JSON

### 1、什么是JSON

- JSON（JavaScript,Object,Ntation,JS对象标记语言）是一种轻量级的数据交换格式，目前使用特别广泛
- 采用完全独立于编程语言的**文本格式**来存储和表示数据
- 简介和清晰的层次结构使得json成为理想的数据交换语言。
- 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率



在JavaScript语言中，一切都是对象。因此，任何javaScript支持的类型都可以通过JSON来表示，例如字符串、数字、对象、数组等。

它的格式为：

- 对象表示为键值对，数据由逗号分隔
- 花括号保存对象
- 方括号保存数组



JSON是javaScript对象的字符串表示法，它使用文本表示一个JS对象的信息，本质是一个字符串。

```javaScript
var obj = {a : "hello" , b : "world"};// 这是一个对象，注意键名也是可以使用引号包裹的
var obj = {"a" : "hello" , "b" : "world"}; // 这是一个	JSON 字符串，本质是一个字符串
```



- JSON和JavaScript互转

  - JavaScript对象转换为JSON

    ```javascript
    // 编写一个JavaScript对象
    var user = {
    name : "龙雨",
    age : 3,
    sex : "男"
    }
    
    // 将js对象转换为 json 对象
    var json = JSON.stringify(user);
    ```

  - JSON转换为JavaScript

    ```javaScript
    // 编写一个Json对象
    var user = {
    "name" : "龙雨",
    "age" : "3",
    "sex" : "男"
    }
    
    // 将JSON对象转换为JavaScript对象
    var obj = JSON.parse(json);
    ```



### 2、控制器返回JSON数据

两种解析工具

1. Jackson  解析工具  （目前比较好的解析工具）
2. 阿里巴巴的fastjson等等

### 3、Jackson

#### 1、导包转格式

1.使用它需要导入她的jar包

```xml
<!--导入json包-->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.9.7</version>
</dependency>
```

2.利用jackson下的ObjectMapper转化对象格式

对象

```java
@RequestMapping("j1")
@ResponseBody // 他就不会走视图解析器，会直接返回一个字符串
public String json1() throws JsonProcessingException {

    // 利用jackson下的ObjectMapper转化格式
    ObjectMapper mapper = new ObjectMapper();

    // 创建一个对象
    User user = new User("龙雨1",3,"男");

    String s = mapper.writeValueAsString(user);
    return s;
}
```

集合

```java
@RequestMapping("j2")
@ResponseBody // 他就不会走视图解析器，会直接返回一个字符串
public String json2() throws JsonProcessingException {

    // 利用jackson下的ObjectMapper转化格式
    ObjectMapper mapper = new ObjectMapper();
    List<User> users = new ArrayList<User>();
    // 创建一个对象
    User user = new User("龙雨1",3,"男");
    User user1 = new User("龙雨2",3,"男");
    User user2 = new User("龙雨3",3,"男");

    users.add(user);
    users.add(user1);
    users.add(user2);

    String s = mapper.writeValueAsString(users);
    return s;
}
```

日期

```java
@RequestMapping("j3")
    @ResponseBody // 他就不会走视图解析器，会直接返回一个字符串
    public String json3() throws JsonProcessingException {

        // 利用jackson下的ObjectMapper转化格式
        ObjectMapper mapper = new ObjectMapper();
        // 不使用时间戳的方式
        mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS,false);
        // 自定义日期格式
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        mapper.setDateFormat(sdf);
        Date date = new Date();

        return mapper.writeValueAsString(date);
    }
```

3、如果是对象返回的是

```
{"name":"龙雨1","age":3,"sex":"男"}
```

4、如果是集合对象返回的是

```html
[{"name":"龙雨1","age":3,"sex":"男"},{"name":"龙雨2","age":3,"sex":"男"},{"name":"龙雨3","age":3,"sex":"男"}]
```

5、如果是日期返回的事

```
"2020-10-23 10:02:01"
```

#### 2、jsonUtils工具类

上面转格式重用那么多操作，我们可以直接使用一个工具类

```java
package com.bdqn.utils;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.SerializationFeature;

import java.text.SimpleDateFormat;

public class JsonUtils {

    public static String getJson(Object object){
        return getJson(object,"yyyy-MM-dd HH:mm:ss");
    }


    public static String getJson(Object object,String dataFormat){
        // 利用jackson下的ObjectMapper转化格式
        ObjectMapper mapper = new ObjectMapper();
        // 不使用时间戳的方式
        mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS,false);
        // 自定义日期格式
        SimpleDateFormat sdf = new SimpleDateFormat(dataFormat);
        mapper.setDateFormat(sdf);
        try {
            return mapper.writeValueAsString(object);
        } catch (JsonProcessingException e) {
            e.printStackTrace();
        }
        return null;
    }

}
```





#### 3、出现乱码问题

配置json乱码（springmvc-servlet.xml）

```xml
<!--JSON乱码问题配置-->
<mvc:annotation-driven>
    <mvc:message-converters register-defaults="true">
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <constructor-arg value="utf-8"/>
        </bean>
        <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
            <property name="objectMapper">
                <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                    <property name="failOnEmptyBeans" value="false"/>
                </bean>
            </property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```



### 4、fastJson

他有三个主要类

- JSONObject代表json对象
- JSONArray代表json对象数组
- JSON代表JSONObject和JSONArray的转化

#### 1、导包

```xml
<!--导入fastjson-->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.47</version>
</dependency>
```

#### 2、转格式

- java对象转json字符串  JSON.toJSONString()
- JSON字符串转java对象  JSON.parseObject(“json字符串”,User.class)
- java对象转json对象  （JSONObject）JSON.toJSON(对象)
- json对象转java对象 (JSON.toJavaObject)JSON.toJSON(对象,User.class)



# 9、SpringMVC的注解

- @Controller  指定这个类
  - 代表这个类会被Spring托管，被这个注解的类，中的所有方法，
  - 如果返回值是String，并且有具体的页面可以跳转，那么就会被视图解析器解析
  - 方法的参数写的是前端接收的数据
- @RequestMapping
  - 如果在类上写就相当于多一层目录
  - 如果是在方法上写就是指定跳转到哪个页面
- @RestController  他下面所有的方法只会返回字符串  （类上写）
- @ResponseBody 指定这个方法返回的是一个字符串
- @PathVariable 设置参数注解  配合RestFul风格
- @RequestParam 指定跟前端的名字一样       设置别名   required 设置是否可以为空
- @RequestBody  把前端字符串传进来
- @RequestHeader 
- @ModelAttribute 在别的方法执行前会先执行   相当于一个作用域
- @SessionAttributes 用于控制器之间的参数共享



这是一套

- @GetMapping
- @PostMapping
- @PutMapping
- @DeleteMapping
- @PatchMapping



# 10、Ajax

### 简介

Ajax是一种在无需加载整个网页的情况下，能够更新部分网页的技术。

Ajax = Asynchronous JacaScript and XML（异步的JavaScript和XML）

语法：

```javascript
 $(function () {
            $("#btn").click(function () {
                $.post({
                    url:"${pageContext.request.contextPath}/a2",
                    success : function (data) {
                        console.log(data);
                    }
                });
            });
        })
```



# 11、拦截器

### 概述：

SpringMVC的处理器拦截器类似于Servlet开发中的过滤器Filter，用于对处理器进行预处理和后处理。开发者可以自己定义一些拦截器来实现特定的功能。

- 过滤器和拦截器的区别：拦截器是AOP思想的具体应用。

过滤器

- servlet规范中的一部分，任何javaWeb工厂都可以使用
- 在url-pattern中配置了/*之后，可以对所有要访问的资源进行拦截

拦截器

- 拦截器是SpringMVC框架自己的，只要实现了SpringMVC框架的工程才能使用
- 拦截器只会拦截访问的控制器方法，如果访问的是jsp/html/css/image/js是不会进行拦截的（也就是静态资源）

### 自定义拦截器

使用步骤：

想要自定义拦截器，必须实现HandlerInterceptor接口

1. 新建一个工程，添加web支持

2. 配置web.xml和spring核心配置文件

3. 编写一个拦截器

   ```java
   package com.bdqn.config;
   
   import org.springframework.web.servlet.HandlerInterceptor;
   import org.springframework.web.servlet.ModelAndView;
   
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   
   public class MyInterceptor implements HandlerInterceptor {
   
       // return true; 执行下一个拦截器，放行
       // return false; 不执行下一个拦截器，放行
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           System.out.println("==================处理前");
           return false;
       }
   
       public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
           System.out.println("====================处理后");
       }
   
       public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
           System.out.println("情理================");
       }
   }
   
   ```

4. 在spring配置文件中加入

   ```xml
   <!--拦截器配置-->
       <mvc:interceptors>
           <mvc:interceptor>
               <mvc:mapping path="/**"/>
               <bean class="com.bdqn.config.MyInterceptor"/>
           </mvc:interceptor>
       </mvc:interceptors>
   ```




# 12、文件上传

### 单服务器上传

1、导入依赖

```xml
 <!--上传依赖-->
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.5</version>
</dependency>
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.3.1</version>
</dependency>
```

2、写form表单

```html
    <h4>文件上传操作：单服务器</h4>
    <form action="${pageContext.request.contextPath}/test/testUpload01" method="post" enctype="multipart/form-data">
        名称：<input type="text" name="fileName">
        图片：<input type="file" name="uploadFile">
        <input type="submit" value="上传">
    </form>
```

3、spring-mvc.xml

```xml
<!--文件上传解析器 id 为固定值-->
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <!--设置上传最大尺寸为-->
        <property name="maxUploadSize">
            <value>52428800</value>
        </property>
    </bean>
```

4、controller

```java
/*单服务器文件上传*/
    @RequestMapping("/testUpload01")
    public String testUpload01(String fileName, MultipartFile uploadFile, HttpServletRequest request) throws IOException {
        // 获取原始文件名
        String name = "";// 定义一个文件名
        String originalFilename = uploadFile.getOriginalFilename();// 获取原始文件名字
        // 获取后缀名
        String suffix = originalFilename.substring(originalFilename.lastIndexOf(".") + 1, originalFilename.length());
        // 使用随机名称，给文件进行命名
        String uuid = UUID.randomUUID().toString().replace("-","");
        // 将名字拼接
        if(!StringUtils.isEmpty(fileName)){
            name = uuid+"_"+fileName+"."+suffix;
        }else{
            name = uuid+"_"+originalFilename;
        }
        // 获取文件路径（判断路径是否存在）
        ServletContext servletContext = request.getSession().getServletContext();
        // 解决服务器文件过多的问题
        String dataPath = new SimpleDateFormat("yyyy-MM-dd").format(new Date());
        String realPath = servletContext.getRealPath("/upload"); // 进行上传文件的路径
        File file = new File(realPath+"/"+dataPath);
        if(!file.exists()){// 判断路径是否存在  不存在就创建
            file.mkdirs();// 创建目录如果该目录下还有子目录也一起创建
        }
        System.out.println(name);
        // 将文件上传
        uploadFile.transferTo(new File(file,name));
        return "success";
    }
```

### 跨服务器上传

1、在以上的基础上再添加两个依赖

```xml
<!--跨服上传依赖-->
<dependency>
    <groupId>com.sun.jersey</groupId>
    <artifactId>jersey-client</artifactId>
    <version>1.19.4</version>
</dependency>
<dependency>
    <groupId>com.sun.jersey</groupId>
    <artifactId>jersey-core</artifactId>
    <version>1.19.4</version>
</dependency>
```

2、写form表单

```html
<h4>文件上传操作：跨服务器</h4>
    <form action="${pageContext.request.contextPath}/test/testUpload02" method="post" enctype="multipart/form-data">
        名称：<input type="text" name="fileName">
        图片：<input type="file" name="uploadFile">
        <input type="submit" value="上传">
    </form>
```

3、tomcat中conf找到web.xml配置文件读写操作

4、controller

```java
 public static final String FILESERVERURL="http://localhost:9090/file/uploads/";
    /*跨服务器文件上传*/
    @RequestMapping("/testUpload02")
    public String testUpload02(String fileName, MultipartFile uploadFile, HttpServletRequest request) throws IOException, NTLMException {

        // 获取原始文件名
        String name = "";// 定义一个文件名
        String originalFilename = uploadFile.getOriginalFilename();// 获取原始文件名字
        // 获取后缀名
        String suffix = originalFilename.substring(originalFilename.lastIndexOf(".") + 1, originalFilename.length());
        // 使用随机名称，给文件进行命名
        String uuid = UUID.randomUUID().toString().replace("-","");
        // 将名字拼接
        if(!StringUtils.isEmpty(fileName)){
            name = uuid+"_"+fileName+"."+suffix;
        }else{
            name = uuid+"_"+originalFilename;
        }

        // 跨服务器路径
        // 创建一个Client对象
        Client client = new Client();
        // 指定上传的路径
        WebResource resource = client.resource(FILESERVERURL+name);
        // 实现上传
        String post = resource.put(String.class, uploadFile.getBytes());
        System.out.println(post);
        return "success";
    }

```



# 13、异常

1、自定义异常类

```java
/**
 * 自定义异常类
 */
public class MyException extends Exception{
    private String message;// 用来保存异常信息

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }

    public MyException(String message) {
        this.message = message;
    }
}

```

2、定义异常处理器

```java

/**
 * 定义异常处理器
 */
public class MyExceptionResolver implements HandlerExceptionResolver {
    public ModelAndView resolveException(HttpServletRequest httpServletRequest,
                                         HttpServletResponse httpServletResponse,
                                         Object o,
                                         Exception e) {
        MyException ex = null;
        if(e instanceof MyException){
            ex = (MyException) e;
        }else{
            ex = new MyException("系统错误，请联系管理员");
        }
        // 用来保存错误信息，跳转视图页面
        ModelAndView mv = new ModelAndView();
        mv.addObject("message",ex.getMessage());
        mv.setViewName("error");
        return mv;
    }
}
```

3、在springmvc中编写

```xml
<!--配置异常处理器-->
    <bean id="myExceptionResolver" class="com.bdqn.exception.MyExceptionResolver"/>
```

4、异常的代码

```java
@Controller
public class MyController {
    @RequestMapping("/testmvc")
    public String testmvc() throws MyException{
        System.out.println("进入控制器");
        try {
            int a = 10/0;
        } catch (Exception e) {
            e.printStackTrace();
            throw new MyException("模拟查询产生异常");
        }
        return "success";
    }
}

```





