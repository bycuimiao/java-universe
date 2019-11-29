1>PriorityQueue是一种无界的，线程不安全的队列  
2>PriorityQueue是一种通过数组实现的，并拥有优先级的队列  
3>PriorityQueue存储的元素要求必须是可比较的对象， 如果不是就必须明确指定比较器  

从源码上看PriorityQueue的入列操作并没对所有加入的元素进行优先级排序。仅仅保证数组第一个元素是最小的即可。  

An unbounded priority {@linkplain Queue queue} based on a priority heap.

不允许add null对象,不允许无法比较的对象(没有实现Comparable接口)

如何去按照顺序遍历PriorityQueue？  
 If you need ordered traversal, consider using {@code Arrays.sort(pq.toArray())}.  
 把PriorityQueue的元素toArray()，然后再排序，也就是api不支持这种遍历
 
注意:PriorityQueue是线程不安全的，如果需要线程安全，可以使用 PriorityBlockingQueue 类

O(log(n)) time for the enqueuing and dequeuing methods ({@code offer}, {@code poll}, {@code remove()} and {@code add});  

linear time for the {@code remove(Object)} and {@code contains(Object)} methods  

constant time for the retrieval methods ({@code peek}, {@code element}, and {@code size})