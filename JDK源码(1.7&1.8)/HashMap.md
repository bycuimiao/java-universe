1、HashMap允许null value and null key。 
Iteration over
 * collection views requires time proportional to the "capacity" of the
 * <tt>HashMap</tt> instance (the number of buckets) plus its size (the number
 * of key-value mappings).  Thus, it's very important not to set the initial
 * capacity too high (or the load factor too low) if iteration performance is
 * important. 
集合视图上的迭代所需的时间与<tt> HashMap </ tt>实例的“容量”（存储桶数）及其大小（键值映射数）成比例。
因此，如果迭代性能很重要，则不要将初始容量设置得过高（或负载因子过低），这一点非常重要。

 Note that using
 * many keys with the same {@code hashCode()} is a sure way to slow
 * down performance of any hash table. To ameliorate impact, when keys
 * are {@link Comparable}, this class may use comparison order among
 * keys to help break ties.  
 请注意，使用许多具有相同{@code hashCode（）}的键是降低任何哈希表性能的肯定方法。 为了改善影响，当键为{@link Comparable}时，此类可以使用键之间的比较顺序来帮助打破平局。
 
  * <p>Note that the fail-fast behavior of an iterator cannot be guaranteed
  * as it is, generally speaking, impossible to make any hard guarantees in the
  * presence of unsynchronized concurrent modification.  Fail-fast iterators
  * throw <tt>ConcurrentModificationException</tt> on a best-effort basis.
  * Therefore, it would be wrong to write a program that depended on this
  * exception for its correctness: <i>the fail-fast behavior of iterators
  * should be used only to detect bugs.</i>
  
  fast-fail机制并不能保证百分百正确触发，不应该依赖异常抛出来保证正确性。
  那么为什么fast-fail机制不能保证百分百正确触发呢？
  我理解是ABA的情况下，会导致触发出现问题，并发修改modCount会导致问题
  
  
2、为什么HashMap node转换为treeNode的阈值是8？
 * Because TreeNodes are about twice the size of regular nodes, we
     * use them only when bins contain enough nodes to warrant use
     * (see TREEIFY_THRESHOLD). And when they become too small (due to
     * removal or resizing) they are converted back to plain bins.  In
     * usages with well-distributed user hashCodes, tree bins are
     * rarely used.  Ideally, under random hashCodes, the frequency of
     * nodes in bins follows a Poisson distribution
     * (http://en.wikipedia.org/wiki/Poisson_distribution) with a
     * parameter of about 0.5 on average for the default resizing
     * threshold of 0.75, although with a large variance because of
     * resizing granularity. Ignoring variance, the expected
     * occurrences of list size k are (exp(-0.5) * pow(0.5, k) /
     * factorial(k)). The first values are:
     *
     * 0:    0.60653066
     * 1:    0.30326533
     * 2:    0.07581633
     * 3:    0.01263606
     * 4:    0.00157952
     * 5:    0.00015795
     * 6:    0.00001316
     * 7:    0.00000094
     * 8:    0.00000006
     * more: less than 1 in ten million  
     https://www.jianshu.com/p/9ad7a192fd7a  
     一般情况下，0.75的load factor，HashMap每个slot可能存在值的几率是0.5，每个HashMap的slot下的Entry个数符合泊松分布，我们只要把Entry下个数k带入泊松分布的公式 (exp(-0.5) * pow(0.5, k) / factorial(k))，就可以得到平常情况（非恶意构造key）下list转tree的概率，如果长度达到8时才转，这种情况发生概率是0.00000006。够低了，所以这个阈值定的8。
     
3、一个树的链表还原阈值为什么是6？
上一小节分析，可以知道，链表树化阀值是8，那么树还原为链表为什么是6而不是7呢？这是为了防止链表和树之间频繁的转换。如果是7的话，假设一个HashMap不停的插入、删除元素，链表个数一直在8左右徘徊，就会频繁树转链表、链表转树，效率非常低下。
    