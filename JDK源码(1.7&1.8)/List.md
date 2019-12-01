ListIterator  
* The <tt>List</tt> interface provides a special iterator, called a
 * <tt>ListIterator</tt>, that allows element insertion and replacement, and
 * bidirectional access in addition to the normal operations that the
 * <tt>Iterator</tt> interface provides.  A method is provided to obtain a
 * list iterator that starts at a specified position in the list.<p>

LinkList没有自己的sort方法，使用的是List接口的default方法，default方法是1.8才加上的，1.7的时候肯定不是这么实现的

jdk8默认排序算法是TimSort，但还保留着老的排序算法legacyMergeSort，并标注未来可能会被删除
TimSort
https://www.jianshu.com/p/892ebd063ad9