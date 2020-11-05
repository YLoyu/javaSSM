> **1、使用MyBatis的mapper接口调用时有哪些要求**

① Mapper接口方法名和mapper.xml中定义的每个sql的id相同；
② Mapper接口方法的输入参数类型和mapper.xml中定义的每个sql 的parameterType的类型相同；
③ Mapper接口方法的输出参数类型和mapper.xml中定义的每个sql的resultType的类型相同；
④ Mapper.xml文件中的namespace即是mapper接口的类路径。

------

> **2、Mybatis的一级、二级缓存**

1）一级缓存: 基于 PerpetualCache 的 HashMap 本地缓存，其存储作用域为 Session，当 Session 刷新或 关闭 之后，该 Session 中的所有 Cache 就将清空，默认打开一级缓存。

2）二级缓存与一级缓存其机制相同，默认也是采用 map存储，不同在于其存储作用域为 Mapper(Namespace)，并且可自定义存储源.  默认不打开二级缓存，要开启二级缓存，使用二级缓存属性类需要实现Serializable序列化接口(可用来保存对象的状态),可在它的映射文件中配置<cache/>或使用注解@CacheNamespace；

3）对于缓存数据更新机制，当某一个作用域(一级缓存 Session/二级缓存Namespaces)的进行了增删改操作后，默认该作用域下所有 查询中的缓存将被 清空掉并重新更新，如果开启了二级缓存，则只根据配置判断是否刷新。

------

> **3、Mybatis是否支持延迟加载？如果支持，它的实现原理是什么？**

Mybatis仅支持association关联对象和collection关联集合对象的延迟加载，association指的就是一对一，collection指的就是一对多查询。在Mybatis配置文件中，可以配置是否启用延迟加载lazyLoadingEnabled=true|false。

它的原理是，使用CGLIB创建目标对象的代理对象，当调用目标方法时，进入拦截器方法，比如调用a.getB().getName()，拦截器invoke()方法发现a.getB()是null值，那么就会单独发送事先保存好的查询关联B对象的sql，把B查询上来，然后调用a.setB(b)，于是a的对象b属性就有值了，接着完成a.getB().getName()方法的调用。这就是延迟加载的基本原理。

------



> **4、为什么说Mybatis是半自动ORM映射工具**

Mybatis在查询关联对象或关联集合对象时，需要手动编写sql来完成，这是非自动化的，数据库的表与实体类的属性的映射关系，是自动化的。

------



> **5、如何获取自动生成的(主)键值**

insert 方法总是返回一个int值 ，这个值代表的是插入的行数。

如果采用自增长策略，在insert标签上使用usegeneratedkeys="true" keyproperty="id" ，自动生成的键值在 insert 方法执行完后可以被设置到传入的参数对象中。

```xml
<insert id="insertname" usegeneratedkeys="true" keyproperty="id">

 	insert into names (name) values (#{name})

</insert>
```

------



> **6、当实体类中的属性名和表中的字段名不一样 ，怎么办**

第1种： 通过在查询的sql语句中定义字段名的别名，让字段名的别名和实体类的属性名一致。

第2种： 通过<resultMap>来映射字段名和实体类属性名的一一对应的关系。

------



> **7、#{}和${}的区别是什么**

\#{}是预编译处理，${}是字符串替换。

Mybatis在处理#{}时，会将sql中的#{}替换为?号，调用PreparedStatement的set方法来赋值；

Mybatis在处理${}时，就是把${}替换成变量的值。

使用#{}可以有效的防止SQL注入，提高系统安全性。

------



> **8、Mybaits的优点，缺点**

Mybaits的优点：

（1）基于SQL语句编程，SQL写在XML里，解除sql与程序代码的耦合；提供XML标签，支持编写动态SQL语句，并可重用。

（2）能够与Spring很好的集成；

（3）提供映射标签，支持对象与数据库的ORM字段关系映射；提供对象关系映射标签，支持对象关系组件维护。

MyBatis的缺点：

（1）SQL语句的编写工作量较大，尤其当字段多、关联表多时，对开发人员编写SQL语句的功底有一定要求。

（2）SQL语句依赖于数据库，导致数据库移植性差，不能随意更换数据库
