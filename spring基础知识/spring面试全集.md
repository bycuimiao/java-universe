###1、spring的事务传播机制
    https://www.cnblogs.com/softidea/p/5962612.html
    https://blog.csdn.net/joenqc/article/details/77141915 
    https://blog.csdn.net/john2522/article/details/64701506
    https://blog.csdn.net/u012833739/article/details/81004631
2、spring事务的隔离级别
3、什么是spring嵌套事务
4、spring循环依赖解决方案
5、如果方法A有事务，在方法A中调用了同类中的B方法，B方法有事务么？
6、代理的方式不同，实现接口是jdk的代理，没有实现的是cglib
7、spring事务注解失效问题
###8、同类a方法没有事务注解，b方法有事务注解，然后在a调用b方法，此时事务是否失效？为什么失效
    因为spring控制事务是通过AOP代理控制的，在a方法调用的时候，没有注解，此时没有使用代理类，而是原类，所以此时b方法的注解失效。
9、spring ioc
###10、spring Bean(IOC 容器)的生命周期
    https://www.cnblogs.com/whuthxb/p/9385761.html
###11、spring Bean的5个作用域
    singleton单例：是spring默认缺省的，全局只有一个对象。
    prototype原型：每次都是新的Bean实例，有状态的Bean建议用此类型。
    request：一次Http请求中，容器返回同一实例Bean，仅在当前Http Request内有效
    session：一次Http Session中，容器返回同一实例Bean，仅在当前Session内有效。
    global session：一个全局的Http Session中，容器返回同一个实例Bean。
###12、jdk代理和cglib代理有什么不一样
    jdk代理：
    需要有顶层接口才能使用，但是在只有顶层接口的时候也可以使用，常见是mybatis的mapper文件是代理。
    使用反射完成。使用了动态生成字节码技术。
    cglib代理：
    可以直接代理类，使用字节码技术，不能对 final类进行继承。使用了动态生成字节码技术。
    jdk和cglib底层原理：
    简单来看就是先生成新的class文件，然后加载到jvm中，然后使用反射，先用class取得他的构造方法，然后使用构造方法反射得到他的一个实例。
###13、依赖注入Bean的属性(类成员变量)有几种方式？
    构造函数注入
    属性setter方法注入
    接口注入
###14、spring支持事务管理的两种方式是哪两种？
    https://blog.csdn.net/u011109589/article/details/80608260
    编程式事务、声明式事务
###15、BeanFactory和ApplicationContext有什么区别
    BeanFactory接口是Spring里面最低层的接口，提供了最简单的容器的功能，只提供了实例化对象和拿对象的功能
    ApplicationContext接口，应用上下文，继承BeanFactory接口，它是Spring的一各更高级的容器，提供了更多的有用的功能；
    1) 国际化（MessageSource）
    2) 访问资源，如URL和文件（ResourceLoader）
    3) 载入多个（有继承关系）上下文 ，使得每一个上下文都专注于一个特定的层次，比如应用的web层  
    4) 消息发送、响应机制（ApplicationEventPublisher）
    5) AOP（拦截器）
###16、@Qualifier注解是做什么用的？
    一个接口有两个实现类，我们要指定其中一个实现类则需要@Qualifier注解
###17、@Autowired和@Resource的区别
    @Autowired默认按照byType方式进行bean匹配，@Resource默认按照byName方式进行bean匹配
    @Autowired是Spring的注解，@Resource是J2EE的注解，这个看一下导入注解的时候这两个注解的包名就一清二楚了
###18、一个接口有两个实现类的时候，怎么注入？
    https://blog.csdn.net/qq_36567005/article/details/80611139
    @Autowired+@Qualifier或者@Resource
###19、@Controller注解下的类，默认是线程安全的么？
    不是。默认scope是singleton，是线程不安全的。@Scope("prototype")设置之后，可以保证每一个请求有一个单独的Action来处理，避免了线程安
    全问题