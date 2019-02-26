#a、基础属性
     	1、初始化容量(DEFAULT_CAPACITY)为10
     	2、
    #b、部分方法逻辑
      	1、add方法
      		先确认是否超过最大容量，若超过list最大容量则先扩容，再将元素add到集合中
      	2、addAll(Collection<? extends E> c)方法
      		将待插入集合c转为数组a
      		确认是否超过最大容量，若超过，则扩容
      		使用System.arraycopy方法将a数组复制到原数组内，并重新计算size
    ![Image text](https://raw.githubusercontent.com/hongmaju/light7Local/master/img/productShow/20170518152848.png)