#《Spring解析bycuimiao》
1、
在spring中看到了一个挺骚的操作，对value值设置为可merg和不可merge，可merge的值会实现Mergeable接口，然后在属性名冲突的时候，
根据是否实现这个接口判断一波，如果可merge就把两个属性merge到一起，不可merge才覆盖
![Image text](https://img-blog.csdnimg.cn/20190112213348928.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI4MTc2MzU=,size_16,color_FFFFFF,t_70)
![Image text](https://img-blog.csdnimg.cn/20190112213657693.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI4MTc2MzU=,size_16,color_FFFFFF,t_70)
这就是一种面向接口的编程，spring很多地方都把这种思想很好的实践了，非常值得我们学习
比如spring的Resource接口把资源全部抽象化了，不管是注解、XML或者其他的配置方式，都是读取完配置之后，通过Resource接口暴露给spring的其他组件

2、
spring有使用catch做逻辑判断
这里，是spring DefaultResourceLoader 中的代码片段，逻辑是location是否可以转换成java.net.URL，这里的配置文件我配置的并非url，所以转换的时候会抛异常进入catch，异常是no protocol,没有这个协议，在catch里面spring直接走了非url的逻辑。
![Image text](https://img-blog.csdnimg.cn/20190106223856433.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI4MTc2MzU=,size_16,color_FFFFFF,t_70)
平时写代码的时候我极力避免这种case，但其实发现简单的在catch中处理一些简单逻辑，也并非不可。有spring给我撑腰，下次再让我check那种不好check的case，我直接catch就ok了，哈哈哈哈

3、
spring Environment对象里面的东西，就是用下面两个方法取到的，没想到异常简单。。。
(Map) System.getProperties()
(Map) System.getenv()
具体的东西大概包括 project的目录，用户所属国家，操作系统版本，jvm版本，环境变量等等系统层面的一些数据
