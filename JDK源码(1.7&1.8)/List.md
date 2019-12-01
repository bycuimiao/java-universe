ListIterator  
* The <tt>List</tt> interface provides a special iterator, called a
 * <tt>ListIterator</tt>, that allows element insertion and replacement, and
 * bidirectional access in addition to the normal operations that the
 * <tt>Iterator</tt> interface provides.  A method is provided to obtain a
 * list iterator that starts at a specified position in the list.<p>

LinkList没有自己的sort方法，使用的是List接口的default方法，default方法是1.8才加上的，1.7的时候肯定不是这么实现的

TimSort
https://www.jianshu.com/p/892ebd063ad9