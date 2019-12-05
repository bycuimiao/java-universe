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


为什么ArrayList是可序列化的，但elementData字段和modCount字段却被transient修饰？
writeObject 和 readObject方法  
序列化时需要使用 ObjectOutputStream 的 writeObject() 将对象转换为字节流并输出。而 writeObject() 方法在传入的对象存在 writeObject() 的时候会去反射调用该对象的 writeObject() 来实现序列化。反序列化使用的是 ObjectInputStream 的 readObject() 方法，原理类似。
是为了保证只序列化实际存储的那些元素，而不是整个数组，从而节省空间和时间。    
为了性能考虑，只序列化里面的元素，空余的槽位不需要序列化。  

ArrayList扩容内存溢出判断条件是 minCapacity < 0 因为Integer.MAX_VALUE + 1溢出后为Integer.MIN_VALUE  
因为int是32位二进制，首位0表示正数，1表示负数  
Integer.MAX_VALUE = 1111111111111111111111111111111
Integer.MIN_VALUE = 10000000000000000000000000000000
在Integer.MAX_VALUE+1之后全部进位，导致首位从0变成1，从最大值变成最小值

ArrayList定义数组最大长度 MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;
ps:但比较疑惑的是，在hugeCapacity()方法中，可以突破ArrayList的最大长度
解释1：该值的设置用于表示数组的最大容量，由于一些虚拟机在数组中会保留一些头字。尝试分配较大的数组可能会导致内存不足错误， 也就是我们常说的内存溢出(VM内存限制)。
Integer.MAX_VALUE-8 其实并不是ArrayList的真实的最大长度，最大长度仍为Integer.MAX_VALUE。 只不过 -8 的真正意义是为了减少不同平台因差异化实现的出错的几率。  
官方注释的意思是：该值的设置用于表示数组的最大容量，由于一些虚拟机在数组中会保留一些头字。尝试分配较大的数组可能会导致内存不足错误， 也就是我们常说的内存溢出(VM内存限制)。
解释2：https://stackoverflow.com/questions/35756277/why-the-maximum-array-size-of-arraylist-is-integer-max-value-8  
##
    The shape and structure of an array object, such as an array of int values, is similar to that of a standard Java object. The primary difference is that the array object has an additional piece of metadata that denotes the array's size. An array object's metadata, then, consists of: Class : A pointer to the class information, which describes the object type. In the case of an array of int fields, this is a pointer to the int[] class.
    
    Flags : A collection of flags that describe the state of the object, including the hash code for the object if it has one, and the shape of the object (that is, whether or not the object is an array).
    
    Lock : The synchronization information for the object — that is, whether the object is currently synchronized.
    
    Size : The size of the array.
    
    max size
    
    2^31 = 2,147,483,648 
    as the Array it self needs 8 bytes to stores the size 2,147,483,648
    
    so
    
    2^31 -8 (for storing size ), 
    so maximum array size is defined as Integer.MAX_VALUE - 8

为了性能考虑，只序列化里面的元素，空余的槽位不需要序列化。


jdk bug  
c.toArray might (incorrectly) not return Object[] (see 6260652)  
https://bugs.java.com/bugdatabase/view_bug.do?bug_id=6260652   
已修复  


trimToSize()方法可以让不变的list节省内存，将数组size设置为list的长度
 
JDK私有方法fastRemove()不做参数校验，节约性能，是私有方法。
对外暴露的public方法有各种校验，而私有方法则没有，这是一个JDK风格的一个细节，也是对public和private的深刻理解  

removeAll()和retainAll()底层用的是同一套逻辑batchRemove()

ArrayList.Itr next()方法,迭代器里面用长度判断ConcurrentModificationException异常的抛出，是觉着只有并发情况，才会出现长度的变化

subList()返回的是ArrayList的一个视图，subList修改会影响源list

ArrayList大部分情况根据modCount == expectedModCount判断是否有并发，并决定是否抛出ConcurrentModificationException异常