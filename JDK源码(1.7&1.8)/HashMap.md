1、HashMap允许null value and null key。 
Iteration over
 * collection views requires time proportional to the "capacity" of the
 * <tt>HashMap</tt> instance (the number of buckets) plus its size (the number
 * of key-value mappings).  Thus, it's very important not to set the initial
 * capacity too high (or the load factor too low) if iteration performance is
 * important. 
集合视图上的迭代所需的时间与<tt> HashMap </ tt>实例的“容量”（存储桶数）及其大小（键值映射数）成比例。
因此，如果迭代性能很重要，则不要将初始容量设置得过高（或负载因子过低），这一点非常重要。