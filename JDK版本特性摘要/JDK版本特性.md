### JDK 9
JDK9更新官方文档：https://docs.oracle.com/javase/9/whatsnew/toc.htm#JSNEW-GUID-BA9D8AF6-E706-4327-8909-F6747B8F35C5  
相关博客：https://www.cnblogs.com/peter1018/p/9209951.html
1、增加jshell  
2、JDK8中接口运行使用静态方法和默认方法后，JDK9可以在接口中使用私有方法  
3、JDK9中钻石操作符(<>)可以使用匿名实现类，可以在匿名实现类中重写方法等操作。  
4、限制使用单独下划线标识符  
5、String 存储结构变更  
从很多不同应用程序收集的信息表名，字符串是堆使用的主要组成部分，而且，大多数字符串对象只包含一个字符，这样的字符只需要一个字节的存储空间，因此这些字符串对象的内部char数组中有一半的空间被闲置。  
JDK9之前String底层使用char数组存储数据private final char value[]，JDK9将String底层存储数据改为byte数组存储数据private final byte[] value。  
StringBuffer和StringBuilder也同样做了变更，将以往char数组改为byte数组。  
6、JDK9在List、Set和Map集合中新增of静态方法，快速创建只读集合。  
7、在Stream接口中新增4个方法：dropWhile、takeWhile、ofNullable,为iterate方法新增重载方法。
8、Optional 类是在JDK8中新增的类，主要是为了解决空指针异常。在JDK9中对这个类进行了改进，主要是新增了三个方法：stream，ifPresentOrElse 和 or 。  
9、在 java.awt.image 包下新增了支持多分辨率图片的API，用于支持多分辨率的图片。  
10、JDK9 中有新的方式来处理 HTTP 调用。它提供了一个新的HTTP客户端(HttpClient)，它将替代仅适用于blocking模式的HttpURLConnection(HttpURLConnection是在HTTP 1.0的时代创建的，并使用了协议无关的方法)，并提供对 WebSocket 和 HTTP/2 的支持。  
   此外，HTTP客户端还提供 API 来处理 HTTP/2 的特性，比如流和服务器推送等功能。  
11、智能 java 编译工具( sjavac )的第一个阶段始于 JEP139 这个项目，用于在多核处理器情况下提升 JDK 的编译速度。如今，这个项目已经进入第二阶段，即 JEP199，其目的是改进 Java 编译工具，并取代目前 JDK 编译工具 javac，继而成为 Java 环境默认的通用的智能编译工具。
   JDK 9 还更新了 javac 编译器以便能够将 java 9 代码编译运行在低版本 Java 中。  
12、日志是解决问题的唯一有效途径：曾经很难知道导致 JVM 性能问题和导致 JVM 崩溃的根本原因。不同的 JVM 日志的碎片化和日志选项（例如：JVM 组件对于日志使用的是不同的机制和规则），这使得 JVM 难以进行调试。
   解决该问题最佳方法：对所有的 JVM 组件引入一个单一的系统，这些 JVM 组件支持细粒度的和易配置的 JVM 日志。  
13、JDK9 的javadoc，现支持HTML5 标准。

### JDK10
1、var 类型推断。可以省去前面的类型声明  
2、GC改进和内存管理 并行全垃圾回收器 G1  
JDK 10中有2个JEP专门用于改进当前的垃圾收集元素。
Java 10的第二个JEP是针对G1的并行完全GC（JEP 307），其重点在于通过完全GC并行来改善G1最坏情况的等待时间。G1是Java 9中的默认GC，并且此JEP的目标是使G1平行。  
3、垃圾回收器接口  
这不是让开发者用来控制垃圾回收的接口；而是一个在 JVM 源代码中的允许另外的垃圾回收器快速方便的集成的接口。    
4、线程-局部变量管控    
5、合并 JDK 多个代码仓库到一个单独的储存库中  
在 JDK9 中，有 8 个仓库： root、corba、hotspot、jaxp、jaxws、jdk、langtools 和 nashorn 。在 JDK10 中这些将被合并为一个，使得跨相互依赖的变更集的存储库运行 atomic commit （原子提交）成为可能。  
6、新增API：ByteArrayOutputStream  

### JDK11
博客：https://www.ibm.com/developerworks/cn/java/the-new-features-of-Java-11/index.html?lnk=hm  
最新发布的 Java11 将带来 ZGC、Http Client 等重要特性，一共包含 17 个 JEP（JDK Enhancement Proposals，JDK 增强提案）。  
1、基于嵌套的访问控制  
与 Java 语言中现有的嵌套类型概念一致, 嵌套访问控制是一种控制上下文访问的策略，允许逻辑上属于同一代码实体，但被编译之后分为多个分散的 class 文件的类，无需编译器额外的创建可扩展的桥接访问方法，即可访问彼此的私有成员，并且这种改进是在 Java 字节码级别的。
  
2、标准 HTTP Client 升级  
Java 11 对 Java 9 中引入并在 Java 10 中进行了更新的 Http Client API 进行了标准化，在前两个版本中进行孵化的同时，Http Client 几乎被完全重写，并且现在完全支持异步非阻塞。    
Java 11 中的新 Http Client API，提供了对 HTTP/2 等业界前沿标准的支持，同时也向下兼容 HTTP/1.1，精简而又友好的 API 接口，与主流开源 API（如：Apache HttpClient、Jetty、OkHttp 等）类似甚至拥有更高的性能。与此同时它是 Java 在 Reactive-Stream 方面的第一个生产实践，其中广泛使用了 Java Flow API，终于让 Java 标准 HTTP 类库在扩展能力等方面，满足了现代互联网的需求，是一个难得的现代 Http/2 Client API 标准的实现
  
3、Epsilon：低开销垃圾回收器  
Epsilon 垃圾回收器的目标是开发一个控制内存分配，但是不执行任何实际的垃圾回收工作。它提供一个完全消极的 GC 实现，分配有限的内存资源，最大限度的降低内存占用和内存吞吐延迟时间。

4、简化启动单个源代码文件的方法  
Java 11 版本中最令人兴奋的功能之一是增强 Java 启动器，使之能够运行单一文件的 Java 源代码。此功能允许使用 Java 解释器直接执行 Java 源代码。源代码在内存中编译，然后由解释器执行。唯一的约束在于所有相关的类必须定义在同一个 Java 文件中。  

5、用于 Lambda 参数的局部变量语法  

6、低开销的 Heap Profiling  
Java 11 中提供一种低开销的 Java 堆分配采样方法，能够得到堆分配的 Java 对象信息，并且能够通过 JVMTI 访问堆信息。  

7、支持 TLS 1.3 协议  
Java 11 中包含了传输层安全性（TLS）1.3 规范（RFC 8446）的实现，替换了之前版本中包含的 TLS，包括 TLS 1.2，同时还改进了其他 TLS 功能，例如 OCSP 装订扩展（RFC 6066，RFC 6961），以及会话散列和扩展主密钥扩展（RFC 7627），在安全性和性能方面也做了很多提升。  

8、ZGC：可伸缩低延迟垃圾收集器  
ZGC 即 Z Garbage Collector（垃圾收集器或垃圾回收器），这应该是 Java 11 中最为瞩目的特性，没有之一。ZGC 是一个可伸缩的、低延迟的垃圾收集器，主要为了满足如下目标进行设计：  
GC 停顿时间不超过 10ms  
即能处理几百 MB 的小堆，也能处理几个 TB 的大堆  
应用吞吐能力不会下降超过 15%（与 G1 回收算法相比）  
方便在此基础上引入新的 GC 特性和利用 colord  
针以及 Load barriers 优化奠定基础  
当前只支持 Linux/x64 位平台  
停顿时间在 10ms 以下，10ms 其实是一个很保守的数据，即便是 10ms 这个数据，也是 GC 调优几乎达不到的极值。根据 SPECjbb 2015 的基准测试，128G 的大堆下最大停顿时间才 1.68ms，远低于 10ms，和 G1 算法相比，改进非常明显。  

9、飞行记录器  
飞行记录器之前是商业版 JDK 的一项分析工具，但在 Java 11 中，其代码被包含到公开代码库中，这样所有人都能使用该功能了。  
Java 语言中的飞行记录器类似飞机上的黑盒子，是一种低开销的事件信息收集框架，主要用于对应用程序和 JVM 进行故障检查、分析。飞行记录器记录的主要数据源于应用程序、JVM 和 OS，这些事件信息保存在单独的事件记录文件中，故障发生后，能够从事件记录文件中提取出有用信息对故障进行分析。  

10、动态类文件常量(字节码级别优化)