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
一个稳定的，自适应的迭代mergesort，在部分排序的数组上运行时，需要远远少于n lg（n）的比较，同时在随机数组上运行时性能堪比传统的mergesort。像所有合适的合并类型一样，这种类型是稳定的，并运行O（n log n）时间（最坏情况）。在最坏的情况下，这种类型需要临时存储空间来存放n / 2个对象引用; 在最好的情况下，它只需要很小的恒定空间。
大体是说，Timsort是稳定的算法，当前排序的数组中已经有排序好的数，它的时间复杂度会小于n logn。与其他合并排序一样，Timesrot是稳定的排序算法，最坏时间复杂在最坏情况下，Timsort算法需要的临时空间是n / 2，在最好情况下，它只需要一个很小的临时存储空间