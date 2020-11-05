# 1、Spring

### 1.1、简介

- Spring：春天------>给软件行业带来了春天
- 2002,首次推出了Spring框架的雏形：interface21框架！
- .Spring框架即以interface21框架为基础,经过重新设计,并不断丰富其内涵,于2004年3月24日,发布了1.0正式
- . Rod Johnson，Spring Framework创始人，著名作者。很难想象Rod Johnson的学历，真的让好多人大吃一惊，他是悉尼大学的博士，然而他的专业不是计算机，而是音乐学。
- spring理念：使现有的技术更加容易使用，本身是一个大杂烩，整合了现有的基础框架！
- SSH :  Struct2 + String + Hibernate!
- SSM :  SpringMVC + Spring + Mybatis

### 1.2、优点

- Spring是一个开源的免费的框架（容器）
- Spring是一个轻量级的、非入侵式的框架
- 控制反转（IOC），面向切面变成（AOP）
- 支持事务的处理，对框架整合的支持

Spring就是一个轻量级的控制反转（IOC）和面向切面编程（AOP）的框架！



### 1.3、组成：七大模块

![image-20201013190057747](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201013190057747.png)

### 1.4、拓展

Spring的官网介绍：现代化的java开发！说白就是基于Spring的开发

![image-20201013190428359](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201013190428359.png)

- Spring Boot
  - 一个快速的脚手架
  - 基于SpringBoot可以快速的开发单个微服务
  - 约定大于配置
- Spring Cloud
  - SpringCloud是基于SpringBoot实现的。



大多数公司都在使用SpringBoot进行快速开发，学习SpringBoot的前提，需要完全掌握Spring及SpringMVC！

### 1.5、什么是spring框架

1. Spring是轻量级的开源的javaEE框架
2. Spring可以解决企业应用开发的复杂性
3. Spring两个核心部分，IOC和AOP
   - IOC：控制反转，把创建对象的过程交给Spring进行管理
   - AOP：面向切面，不修改源代码进行功能增强

### 1.6、spring特点

1. 方便解耦，简化开发
2. Aop
4. 方便和其他框架整合
5. 方便进行事务操作
6. 既降低javaEEAPI开发难度
7. java源码是经典学习范例

### 1.7、注意三个实现类

![image-20201019093702621](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201019093702621.png)

- AnnotationConfigApplicationContext
  - 注解开发
- FileSystemXmlApplicationContext
  - 磁盘路径获取配置文件
- ClassPathXmlApplicationContext
  - src类路径获取配置文件

# 2、IOC理论推导

我们原来的代码需要，根据用户的需求要更改源码

控制反转

让用户自己设置需求

- 之前，程序是主动创建对象！控制权在程序员手上
- 使用了set注入后，程序不再具有主动性，而是变成了被动的接受对象！

这种思想，从本质上解决了问题，我们程序员不用再去管理对象的创建了。系统的耦合性大大降低~，可以更加专注的在业务的实现上！这时IOC的原型！

控制反转就是把程序交给spring



### IOC底层实现原理

1. 底层实现使用的技术
   1.1 xml配置文件
   1.2 dom4j解析xml
   1.3 工厂模式
   1.4 反射



### IOC本质

**控制反转loC(Inversion of Control)，是一种设计思想，DI(依赖注入)是实现loC的一种方法，**

也有人认为DI只是loC的另一种说法。没有loC的程序中，我们使用面向对象编程，对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是︰获得依赖对象的方式反转了。



采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。

**控制反转是一种通过描述（(XML或注解）并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是loC容器，其实现方法是依赖注入(Dependency Injection,Dl)。**

# 3、helloSpring

配置文件applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:c="http://www.springframework.org/schema/c"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:aop="http://www.springframework.org/schema/aop" 
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd
">

    <bean id="..." class="...">  
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions go here -->

     <!-- 开启注解支持 -->
    <context:annotation-config/>
</beans>
```

# 4、IOC创建对象的方式

1. 使用无参构造创建对象，默认

2. 假设我们要使用有参构造创建对象。

   1. 下标赋值

      ```xml
      <!--下标赋值-->
      <bean id="user" class="com.bdqn.entity.User">
          <constructor-arg index="0" value="龙雨"/>
      </bean>
      ```

   2. 通过类型赋值

      ```xml
       <!--不建议使用-->
       <bean id="user" class="com.bdqn.entity.User">
       	<constructor-arg type="java.lang.String" value="龙雨"/>
       </bean>
      ```

   3. 通过参数名

      ```xml
      <bean id="user" class="com.bdqn.entity.User">
      	<constructor-arg name="name" value="龙雨"/>
      </bean>
      ```



总结：在配置文件加载的时候，容器中管理的对象就已经初始化了！



# 5、Spring配置

### 别名

```xml
<!--别名：如果添加了别名，也可以通过使用别名获取到对象-->
    <alias name="user" alias="asdasd"/>
```

### Bean的配置

1. id : bean的唯一标识符，也就是相当于我们学的对象名
2. class : bean对象所对应的全限定名 ： 包名+类名
3. name : 也是别名,而且name 可以同时取多个别名,可以通过多个分隔符使用

```xml
<bean id="user" class="com.bdqn.entity.User" name="user2,u2 u3;u4">
	<property name="name" value="龙雨"/>
</bean>
```

### import

import,一般用于团队开发使用，他可以将多个配置文件，导入合并为一个

就是将配置文件合成一个主配置文件

- 例如将beans.xml导入到applicationContext.xml中

```xml
<import resource="beans.xml"/>
```

使用的时候，直接使用总的配置文件就可以

- 如果内容相同的话，自动合并；

  

# 6、依赖注入

### 构造器注入

- 转IOC理论推导

### Set方式注入【重点】

- 依赖注入：本身是set注入！
  - 依赖：bean对象的创建依赖于容器
  - 注入：bean对象中的所有属性，由容器来注入！

**环境搭建**

1. 复杂类型

   ```java
      private String address;
   
       public String getAddress() {
           return address;
       }
   
       public void setAddress(String address) {
           this.address = address;
       }
   
       @Override
       public String toString() {
           return "Address{" +
                   "address='" + address + '\'' +
                   '}';
       }
   ```

2. 真实测试对象

   ```java
   public class Student {
       private String name;
       private Address address;
       private String[] books;
       private List<String> hobbys;// 爱好
       private Map<String,String> card;
       private Set<String> games;
       private String wife;
   }
   ```

3. applicationContext.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <bean id="address" class="com.bdqn.pojo.Address">
           <property name="address" value="邯郸"/>
       </bean>
       <bean id="student" class="com.bdqn.pojo.Student">
           <!--第一种，普通值注入，value-->
           <property name="name" value="龙雨"/>
   
           <!--第二种，bean注入，ref-->
           <property name="address" ref="address"/>
   
           <!--数组-->
           <property name="books">
               <array>
                   <value>西游记</value>
                   <value>红楼梦</value>
                   <value>水浒传</value>
                   <value>三国演义</value>
               </array>
           </property>
   
           <!--list-->
           <property name="hobbys">
               <list>
                   <value>听歌</value>
                   <value>玩游戏</value>
               </list>
           </property>
   
           <!--Map-->
           <property name="card">
               <map>
                   <entry key="身份证" value="12313232313132"/>
                   <entry key="银行卡" value="5454545"/>
               </map>
           </property>
   
           <!--set-->
           <property name="games">
               <set>
                   <value>王者荣耀</value>
                   <value>LOL</value>
                   <value>cf</value>
               </set>
           </property>
   
           <!--null-->
           <property name="wife">
               <null/>
           </property>
   
           <!--Properties-->
           <property name="info">
               <props>
                   <prop key="driver">20190925</prop>
                   <prop key="url">男</prop>
                   <prop key="username">root</prop>
                   <prop key="password">longyu..</prop>
               </props>
           </property>
       </bean>
   </beans>
   ```

4. 测试类

   ```java
   public class Test {
       public static void main(String[] args) {
           ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
           Student student = (Student) context.getBean("student");
           System.out.println(student);
       }
   }
   ```

5. 结果

   ```java
   Student{name='龙雨', 
           address=Address{address='null'}, 
           books=[西游记, 红楼梦, 水浒传, 三国演义], 
           hobbys=[听歌, 玩游戏], 
           card={身份证=12313232313132, 银行卡=5454545},
           games=[王者荣耀, LOL, cf], wife='null', 
           info={password=longyu.., url=男, driver=20190925, username=root}
   }
   
   ```

   

### 拓展方式注入

我们可以使用P命名空间和c命名空间进行注入

例：

- beans.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:c="http://www.springframework.org/schema/c"
         xmlns:p="http://www.springframework.org/schema/p"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          https://www.springframework.org/schema/beans/spring-beans.xsd">
  
      <!--p命名注入，可以直接注入属性的值：property-->
      <bean id="user" class="com.bdqn.pojo.User" p:name="龙雨" p:age="15"/>
      <!--c命名注入，通过构造器 construct-ages-->
      <bean id="user2" class="com.bdqn.pojo.User" p:name="long" p:age="15"/>
  </beans>
  ```

- User类

  ```java
  public class User {
      private String name;
      private Integer age;
  
      public String getName() {
          return name;
      }
  
      public void setName(String name) {
          this.name = name;
      }
  
      public Integer getAge() {
          return age;
      }
  
      public void setAge(Integer age) {
          this.age = age;
      }
  }
  ```

- 测试类

  ```java
    @Test
      public void test2(){
          ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
          User user = (User) context.getBean("user");
          System.out.println(user.getName()+user.getAge());
  
          user  = (User) context.getBean("user2");
          System.out.println(user.getName()+user.getAge());
      }
  ```

注意：p命名和c命名空间不能直接使用，需要导入xml约束！

### bean的作用域

| Scope                                                        | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [singleton](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#beans-factory-scopes-singleton) | （默认值）将每个Spring IoC容器的单个bean定义范围限定为单个对象实例。 |
| [prototype](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#beans-factory-scopes-prototype) | 将单个bean定义的作用域限定为任意数量的对象实例。             |
| [request](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#beans-factory-scopes-request) | 将单个bean定义的范围限定为单个HTTP请求的生命周期。 也就是说，每个HTTP请求都有一个自己的bean实例，它是在单个bean定义的后面创建的。 仅在可感知网络的Spring ApplicationContext中有效。 |
| [session](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#beans-factory-scopes-session) | 将单个bean定义的作用域限定为HTTP`Session`的生命周期。 仅在可感知网络的Spring ApplicationContext中有效。 |
| [application](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#beans-factory-scopes-application) | 将单个bean定义的作用域限定为ServletContext的生命周期。 仅在可感知网络的Spring ApplicationContext中有效。 |
| [websocket](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/web.html#websocket-stomp-websocket-scope) | 将单个bean定义的作用域限定为WebSocket的生命周期。 仅在可感知网络的Spring ApplicationContext中有效。 |

![image-20201015194042845](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201015194042845.png

1. 单例模式（Spring默认机制）

   ```xml
   <bean id="accountService" class="com.something.DefaultAccountService" scope="singleton"/>
   ```

2. 原型模式：每次从容器中get的时候，都会产生一个新对象！

   ```xml
   <bean id="accountService" class="com.something.DefaultAccountService" scope="prototype"/>
   ```

3. 其余的request、session、application这些只能在web开发用到！

# 7、bean的自动装配

- 自动装配是Sping满足bean依赖一种方式
- Sping会在上下文中自动寻找，并自动给bean装配属性！

Spring中有三种装配的方式

1. 在xml中显示的配置
2. 在java中显示配置
3. 隐式的自动装配bean【重要】

### 测试

1. 环境搭建：一个人有两只宠物

### byName自动装配

```xml
<bean id="cat" class="com.bdqn.pojo.Cat"/>
<bean id="dog" class="com.bdqn.pojo.Dog"/>
<!--
    autowire : 自动装配
    byName : 会自动在容器上下文中查找，和自己对象set方法后面的值对应的beanid！
    -->
<bean id="people" class="com.bdqn.pojo.People" autowire="byName">
    <property name="name" value="小明"/>
</bean>
```

### byType自动装配

```xml
<bean id="cat" class="com.bdqn.pojo.Cat"/>
<bean id="dog" class="com.bdqn.pojo.Dog"/>

<!--
    autowire : 自动装配
    byType : 会自动在容器上下文中查找，和自己对象属性类型相同的beanid！
    -->
<bean id="people" class="com.bdqn.pojo.People" autowire="byType">
    <property name="name" value="小明"/>
</bean>
```

小结：

- byName的时候，需要保证bean的id唯一，并且这个bean需要和自动注入的属性的set方法的值一致（也就是类名跟id名一样）
- byType的时候，需要保证bean的class唯一，并且这个bean需要和自动注入的属性类型一致（也就是说类里面的属性类型只能有一个，不能重复）

### 使用注解自动装配

基于注释的配置的引入提出了一个问题，即这种方法是否比XML“更好”。

要使用注解须知：

1. 导入约束。context约束

2. 配置注解的支持：<context:annotation-config/>

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           https://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/context
           https://www.springframework.org/schema/context/spring-context.xsd">
   
       <!-- 开启注解支持 -->
       <context:annotation-config/>
   
   </beans>
   ```

@Autowried

直接在属性上使用即可！也可以在set方式上使用

注：使用Autowired我们可以不用编写set方法，前提是你这个自动装配的属性在IOC（Spring）容器中存在，且符合名字ByName！

科普：

```
@Nullable    字段标记了这个注解，说明这个字段可以为null
```

如果@Autowried自动装配的环境比较复杂，自动装配无法通过一个注解【@Autowired】完成的时候，我们可以使用@Qualifier(value=“xxx”)去配置@Autowired的使用，指定一个唯一的bean对象注入！

```java
public class People {
    @Autowired
    @Qualifier(value = "cat1")
    private Cat cat;
    @Autowired
    @Qualifier(value = "dog1")
    private Dog dog;
}
```



@Resource注解

```java
public class People {
    @Resource(name = "cat1")
    private Cat cat;
    @Autowired
    @Resource(name = "dog1")
    private Dog dog;
 }
```



小结：

@Resource和@Autowired的区别：

- 都是用来自动装配的，都可以放在属性字段上
- @Autowired 通过byType的方式实现【常用】，如果找不到名字，则通过byName实现！
- @Resource 默认通过byName的方式实现，如果找不到名字，则通过byType实现！如果两个都找不到的情况下，就报错！【常用】
- 执行顺序不同：@Autowired 通过byType的方式实现.@Resource 默认通过byName的方式实现，



# 8、使用注解开发

在Spring4之后，要使用注解开发，必须要保证aop的包导入了

![image-20201016093835393](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201016093835393.png)

使用注解需要导入context约束，增加注解的支持！

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
">
    
	<!--指定要扫描的包，这个包下的注解就会生效-->
    <context:component-scan base-package="com.bdqn"/>
    <!-- 开启注解支持 -->
    <context:annotation-config/>
</beans>
```



1. bean

2. 属性如何注入

   1. ```xml
      // 等价于<bean id="user" class="com.bdqn.pojo.User"/>
      // @Component组件
      @Component
      public class User {
          public String name;
          // 相当于 <property name = "name" value="龙雨"></property>
          @Value("龙雨1")
          public void setName(String name) {
              this.name = name;
          }
      }
      ```

      

3. 各种注解和衍生的注解

   - @Autowired作用：自动按照类型注入。只要容器中有唯一的一个bean对象类型和要注入的变量类型匹配，就可以注入成功！
     - 出现位置：可以是变量上，也可以是方法上！
     - 细节：在使用注解注入时，set方法就不是必须的了。
     - 如果ioc容器中没有任何bean的类型和要注入的变量类型匹配，则报错
     - 如果ioc容器中有多个类型匹配时：
   - @Qualifier作用: 在按照类型注入的基础之上再按照名称注入。它在给类成员注入时不能单独使用。但是再给方法参数注入时可以！
     - 属性:value:用于指定注入bean的id
   - @Resource作用：直接按照bean的id注入。他可以独立使用
     - 属性：name：用于指定bean的id
   - @Value作用：用于注入基本数据类型和String类型的数据
   - Spring新注解在不写配置文件的情况下可以使用
     - @Configuration：指定当前类是一个配置类
     - @ComponentScan：用于通过注解指定spring在创建容器时要扫描的包

   

   

   注意：上面三个注解只能注入其他bean类型的数据，而基本数据和String类型无法使用上述注解实现。另外，集合类型的注入只能通过xml来实现

   

   

   - @Component有几个衍生注解，我们在web开发中 ，会按照mvc三层架构分层！
     - dao【@Repository】 
     - service【@Service】
     - controller 【@Controller】

   这四个注解功能都是一样的，都是代表将某个类注册到Spring中，装配Bean

4. 自动装配置

   ```
   @Autowired :自动装配通过类型。名字
   如果Autowired不能唯一自动装配上属性，则需要通过@Qualifier(value="xxx")
   @Nullable字段标记了这个注解，说明这个字段可以为null;
   @Resource:自动装配通过名字。类型。
   ```

5. 作用域

   @Score：注解在类上（用于指定bean的作用范围）

   参数为：

   - prototype 原型
   - request
   - session  
   - singleton 单例

6. 小结

   xml与注解

   - xml更加万能，适合于任何场合！维护简单方便
   - 注解不是自己的类用不了，维护相对复杂

   xml与注解最佳实践：

   - xml用来管理bean
   - 注解只负责属性的注入
   - 我们在使用的过程中，只需要注意必须让注解生效，就需要开启注解的支持



# 9、使用java的方式配置Spring

我们现在要完全不用使用Spring的xml配置，全权交给java来做！

JavaConfig是Spring的一个子项目，在Spring4之后，它成为了一个核心功能！

- @Configuration指定这是一个配置类
- @ComponentScan用于通过注解指定spring在创建容器时要扫描的包
- @Bean用于把当前方法的返回值作为bean对象存入spring的ioc容器中
  - 属性 name:用于指定bean的id。当不写时，默认值时当前方法的名称

### 配置文件类

```java
// 这个也会被Spring容器托管，注册到容器中，因为他本来就是一个@Component
// @Configuration代表就是一个配置类，就和我们之前看的beans.xml
@Configuration
@ComponentScan("com.bdqn.pojo")
public class MyConfig {

    // 注册一个bean，就相当于我们之前写的bean标签，
    // 这个方法的名字就相当bean标签中的id属性
    // 这个方法的返回值，就相当于bean标签中的class属性
    @Bean
    public User user(){
        return new User();// 就是返回要注入到bean的对象
    }

}
```

### 实体类

```java
// 这个注解的意思，就是说明这个类被Spring接管了，注册到了容器中
@Controller
public class User {
    @Value("龙雨")
    private String name;

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

```

### 测试类

```java
public class MyTest {
    public static void main(String[] args) {
        // 如果完全使用了配置类去做，我们就只能通过AnnotationConfig上下文来获取容器，通过配置类的class对象加载！
        ApplicationContext context = new AnnotationConfigApplicationContext(MyConfig.class);
        User user = (User) context.getBean("user");
        System.out.println(user.getName());
    }
}
```

这种纯java的配置方式，在SpringBoot中随处可见！

# 10、代理模式

为什么要学习代理模式？

因为这就是SpringAOP的底层！【SpringAOP和SpringMVC】

代理模式的分类：

- 静态代理
- 动态代理

![image-20201016105533309](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201016105533309.png)

### 静态代理

角色分析：

- 抽象角色：一般会使用接口或者抽象类来解决
- 真实角色：被代理的角色
- 代理角色：代理真实角色，代理真实角色后，我们会一般会做一些附属的操作
- 客户：访问代理对象的人！



代理模式的好处：

- 可以使真实角色的操作更加纯粹！不用去关注一些公关的业务
- 公共业务就交给了代理角色，实现了业务的分工！
- 公共业务发生扩展的时候，方便集中管理！

缺点：

- 一个真是角色就会产生一个代理角色，代码量会翻倍，开发效率会变低



### 动态代理

- 动态代理和静态代理角色一样

- 动态代理的代理类是动态代理的，不是我们直接写好的！

- 动态代理分文两大类：基于接口的动态代理，基于类的动态代理

  - 基于接口---JDK动态代理【我们在这里使用】
  - 基于类：cglib
  - java字节码实现：javasist

   

需要了解两个类：proxy 代理，InvocationHandler：调用处理程序

```java
public class ProxyInvocationHandler implements InvocationHandler {
    // 被代理的接口
    private Object target;

    public void setTarget(Object target) {
        this.target = target;
    }

    // 生成代理类
    public Object getProxy(){
        return Proxy.newProxyInstance(this.getClass().getClassLoader(),target.getClass().getInterfaces(),this);
    }

    // 执行被代理对象的任何接口都会经过该方法：下面写参数的含义
    // 处理代理实例并返回结果
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        // 动态代理的本质就是使用反射机制实现
        // 这里写提供增强的代码
        Object result = method.invoke(target, args);
        return result;
    }

}

```

上面参数的含义：

1. proxy:代理对象的引用
2. method:当前执行的方法
3. args :当前执行方法所需的参数
4. return:和被代理对象方法有相同的返回值

动态代理的好处：

- 可以使真实角色的操作更加纯粹！不用去关注一些公关的业务
- 公共业务就交给了代理角色，实现了业务的分工！
- 公共业务发生扩展的时候，方便集中管理！
- 一个动态代理类代理的是一个接口，一般就是对应的一类业务
- 一个动态代理类可以代理多个类，只要是实现了同一个接口



# 11、AOP

### 什么是AOP

AOP（Aspect Oriented Programming）意为：面向切面变成，通过预编译方式和运行期动态代理实现程序功能的统一维护的技术。AOP是OOP的延续，是软件开发的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生泛型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。





### AOP的作用和优势

作用：在程序运行期间，不修改源码对已有方法进行增强。

优势：减少重复代码，提高开发效率，维护方便





### AOP相关的术语

- Joinpoint(链接点)：所谓连接点是指那些被拦截到的点，在spring中，这些点指的是方法，因为spring只支持方法类型的连接点。
- Point(切入点)：所谓切入点是指我们要对哪些Joinpoint进行拦截的定义。



1、关注点(concern)

1) 核心关注点: 关注系统的业务逻辑 --> OOP

2) 横切关注点: 关注系统级服务,比如事务、安全、日志 --> AOP

2、切面(aspect):

 把散落在系统各处与横切关注点相关的重复代码抽取出来归整到一处形成一个模块,我们称为方面.

3、连接点(joinpoint):

 程序运行过程中的某一点.比如方法调用、属性访问、异常抛出.

4、切入点(pointcut):  一组连接点

 注意: 如果要有选择性地拦截目标对象中的方法的话需要定义切入点

5、增强(advice)也就是通知:

通知：

- 前置通知@Before：方法前执行
- 后置通知@AfterReturning：方法后执行（目标方法没有异常，执行有异常，不执行）
- 异常通知@AfterThrowing：方法发生异常时执行
- 最终通知@After：相当于finally，不论是否发生异常都执行
- 环绕通知@Around：前置，后置，异常，最终，四大增强方法的结合

 在不修改原有代码的前提下,为某一个对象增加新的功能

 (如:事务服务、日志服务),在spring中增强是通过拦截器实现的.

6、织入(Weaving): 

  将方面加入到(拦截器)方法中为对象增加额外功能的过程称为织入

7、目标对象(target object): 需要被增强功能的对象称之为目标对象,也被称为被增强或被代理对象。

​     在spring中通常指service层接口实现类的对象

8、代理对象(proxy object)

 为目标对象增加新功能从而产生的一个新的对象称为代理对象.负责调用拦截器和目标对象的方法.

9、拦截器

 1) 前增强拦截器

在目标对象方法执行之前,执行此拦截器为目标对象增加新功能

实现接口: MethodBeforeAdvice

 2) 后增强拦截器

在目标对象方法执行之后,执行此拦截器为目标对象增加新功能

实现接口: AfterReturningAdvice

 3) 环绕增强拦截器

在目标对象方法执行前后,执行此拦截器为目标对象增加新功能

实现接口: MethodInterceptor

 4) 抛出增强拦截器

在目标对象方法抛出异常后,执行此拦截器为目标对象增加新功能

实现接口: ThrowsAdvice

定义方法: 

public void afterThrowing

([Method method], [Object[] args], [Object target], Throwable subclass)

10、增强器(advisor)

为拦截器定义切入点(一组连接点)之后产生增强器,增强器可以有选择性地拦截目标对象中的部分方法.

注意: 拦截器默认拦截所有目标对象中的方法

spring框架中的增强器:

org.springframework.aop.support.RegexpMethodPointcutAdvisor

method="" destroy-method=""/>



### Aop在Spring中的作用

- 提供声明式事务;允许用户自定义切面
- ·横切关注点:跨越应用程序多个模块的方法或功能。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志，安全，缓存，事务等等...
- ·切面(ASPECT):横切关注点被模块化的特殊对象。即，它是一个类。
- ·通知(Advice):切面必须要完成的工作。即，它是类中的一个方法。
- ·目标(Target):被通知对象。
- ·代理（Proxy):向目标对象应用通知之后创建的对象。
- ·切入点(PointCut):切面通知执行的“地点"的定义。
- ·连接点(JointPoint):与切入点匹配的执行点。

### 使用Spring实现AOP（独立的框架）

【重点】使用AOP植入，需要导入一个依赖包！

```xml
 <dependency>
     <groupId>org.aspectj</groupId>
     <artifactId>aspectjweaver</artifactId>
     <version>1.9.6</version>
 </dependency>
```

方式一：使用Spring的接口【主要SpingAPI接口实现】

```java
	/**
     * 环绕通知，一般单独使用
     * @return
     */
    public Object around(ProceedingJoinPoint point){
        try {
            System.out.println("前置增强");
            Object o = point.proceed();// 让切入点方法继续执行
            System.out.println("后置增强");
            return o;
        }catch (Throwable throwable){
            System.out.println("异常通知");
            throwable.getMessage();
            return null;
        }finally {
            System.out.println("最终通知");
        }
    }
```

方式二：自定义来实现AOP【主要是切面定义】

方式三：使用注解实现

以上三种对应spring_demo06AOP

### JoinPoint对象

joinPoing是aspectj包下的一个类

每个通知方法都有一个joinpoint类

通过这个类可以获取

- joinPoint.getSignature().getName();目标方法名
- joinPoint.getSignature().getDeclaringType();类名
- joinPoint.getSignature().getDeclaringTypeName();全限定类名
- Modifier.toString(joinPoing.getSignature().getModifiers());方法声明类型

# 12、整合mybatis

步骤：

1. 导入相关jar包
   - junit
   - mybatis
   - mysql数据库
   - spring相关的
   - aop植入
   - mybatis-spring【new】:整合mybatis和spring的
2. 编写配置文件
3. 测试

### 回忆mybatis

1. 编写实体类
2. 编写核心配置文件
3. 编写接口
4. 编写Mapper.xml
5. 测试



#### mybatis-spring

1. 编写数据源配置
2. sqlSessionFactory
3. sqlSessionTemplate
4. 需要给接口加实现类
5. 将自己写的实现类，注入到spring中
6. 测试使用





# 13、声明式事务

### 1、回顾事务

- 把一组业务当成一个业务来做；要么都成功，要么都失败
- 事务在项目开发中，十分的重要，涉及到数据一致性问题，不能马虎！
- 确保完整性一致性！



事务的ACID原则：Atomicity Consistency Isolation Durability

- 原子性
  - 将sql语句作为一个整体，要么都成功，要么都失败
- 一致性
  - 操作之前和操作之后的总量是不变的
- 隔离性
  - 多个业务可能操作同一个资源，防止数据损坏
- 持久性
  - 事务一旦提交无论系统发生什么问题，结果都不会再被影响，被持久化的写到存储器中！



### 2、spring中的事务管理

- 声明式事务：AOP【一般都用这个】
- 编程时事务：需要在代码中，进行事务的管理





# 14、bean的生命周期

### 单例对象：

- 出生：当容器创建时对象出生
- 活着：只要容器还在，对象就一直活着
- 死亡：容器销毁，对象消亡
- 总结：单例对象的生命周期和容器相同

### 多例对象：

- 出生：当我们使用对象时spring框架为我们创建
- 活着：对象只要在使用过程中就一直活着
- 死亡：当对象长时间不用，且没有别的对象引用时，由java的垃圾回收期回收























