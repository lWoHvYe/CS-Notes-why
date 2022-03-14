## MyBatis

- Mybatis是一个半ORM（对象关系映射）框架

- Mapper 接口是没有实现类的，当调用接口方法时，接口全限名+方法名拼接字符 串作为 key 值，可唯一定位一个 MapperStatement。在xml中，通过namespace + id 唯一对应一个MapperStatement

- Mybatis支持通过插件进行扩展，比如分页插件

- Mybatis 提供了 9 种动态 sql 标签:trim | where | set | foreach | if | choose | when | otherwise | bind

- Mybaits常用的标签：select | insert | update | delete | resultMap | parameterMap | sql | include | selectKey | 动态sql标签

- 一对一、一对多的配置：association一对一、collection一对多

- Mybatis默认开启一级缓存（SqlSession级别）、另外还有二级缓存（namespace级别，可自定义存储源）。都是基于 PerpetualCache 的 HashMap 本地缓存

- 简述 Mybatis 的插件运行原理，以及如何编写一个插件

  Mybatis 仅可以编写针对 ParameterHandler、ResultSetHandler、 StatementHandler、Executor 这 4 种接口的插件，Mybatis 使用 JDK 的动态代理，为需要拦截的接口生成代理对象以实现接口方法拦截功能，每当执行这 4 种 接口对象的方法时，就会进入拦截方法，具体就是 InvocationHandler 的 invoke() 方法，当然，只会拦截那些你指定需要拦截的方法。

  编写插件:实现 Mybatis 的 Interceptor 接口并复写 intercept()方法，然后再给插件编写注解，指定要拦截哪一个接口的哪些方法即可，记住，别忘了在配置文件中配置你编写的插件。

- 