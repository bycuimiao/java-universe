#a、基础属性
  ##1、初始化容量(DEFAULT_CAPACITY)为10
     	#2、
#b、部分方法逻辑
  ##1、add方法
      		先确认是否超过最大容量，若超过list最大容量则先扩容，再将元素add到集合中
  ##2、addAll(Collection<? extends E> c)方法
      		将待插入集合c转为数组a
      		确认是否超过最大容量，若超过，则扩容
      		使用System.arraycopy方法将a数组复制到原数组内，并重新计算size
    
![Image text](https://github.com/bycuimiao/java-universe/blob/master/img/1401551165786_.pic_hd.jpg)
  ##3、remove(Object o)
 				判断o是否为null，若为null则移除第一个为null的元素，否则使用equals方法找到相等元素并移除(fastRemove)
 				移除时逻辑：自增修改次数modCount(并发修改时根据这个字段进行判断并fastfail，注意，在新增元素导致扩容时候也会对modCount进行自增)，
  ##2、扩容方法
#b、线程安全