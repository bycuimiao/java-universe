![Image text](https://img-blog.csdnimg.cn/20190112213348928.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI4MTc2MzU=,size_16,color_FFFFFF,t_70)

### 为什么HashMap的长度是2的n次幂？
	答:因为元素存放位置是根据计算key的哈希值来确定的，正常的思路是确定哈希值后对数组长度取模来确定具体放在数组的那个位置。但取模的计算耗时
	较长，所以jdk使用了&来计算具体位置，h & (length-1)，而&符的计算，如果length不为2的n次幂，会有一些数组的位置被浪费，永远无法取到，所
	以不管你指定长度为多少，hashmap都会自动给你把长度设置为2的n次幂。具体见博客：http://www.importnew.com/16301.html