#《Spring解析bycuimiao》
在spring中看到了一个挺骚的操作，对value值设置为可merg和不可merge，可merge的值会实现Mergeable接口，然后在属性名冲突的时候，
根据是否实现这个接口判断一波，如果可merge就把两个属性merge到一起，不可merge才覆盖
![Image text](https://img-blog.csdnimg.cn/20190112213348928.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI4MTc2MzU=,size_16,color_FFFFFF,t_70)
![Image text](https://img-blog.csdnimg.cn/20190112213657693.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI4MTc2MzU=,size_16,color_FFFFFF,t_70)
这就是一种面向接口的编程，spring很多地方都把这种思想很好的实践了，非常值得我们学习
比如spring的Resource接口把资源全部抽象化了，不管是注解、XML或者其他的配置方式，都是读取完配置之后，通过Resource接口暴露给spring的其他组件