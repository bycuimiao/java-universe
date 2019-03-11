#《Spring编程风格》
1、xxxToUse
![Image text](https://img-blog.csdnimg.cn/20190106225356418.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI4MTc2MzU=,size_16,color_FFFFFF,t_70)
例如，这里，path是入参，但经过处理后spring并非是将path覆盖掉，而是用pathToUse接住新的参数，这种情景在spring源码中多次出现，吾辈视为spring风格之一
