## Java-容器

### 集合

- Set（集）、List（列表）、Map（映射）

- 数组、链表、二叉树（二叉搜索树、红黑树）、堆、栈、图

- 先通过HashCode判断，相同再用equals判断

- ArrayList扩容为1.5倍。int newCapacity = oldCapacity + (oldCapacity >> 1)

- HashMap

    - 1.8之前：数组 + 链表
    - 1.8开始：数组 + 链表 + 红黑树
    - 容量始终保持为2^n，扩容时增大一倍，初始及扩容的源码（判断容量是否达到最大、重新hash，针对rehash进行了优化：位运算的一些结果，主体上若(e.hash & oldCap) == 0，则放在index桶中，否则放在index + oldCap中）
    - 容量保持2^n，是为了使用这个位运算的性质：若x为2的n次幂，则 y%x 等同于 y&(x-1)

- ConcurrentHashMap

    - 1.8之前：Segment段 （ReentrantLock加锁），段数（并行度）默认16，初始化后不会改变

      结构：Segment 数组 + HashEntry 数组 + 链表

    - 1.8开始：使用的 Synchronized 锁加 CAS 的机制

      结构：Node 数组 + 链表 / 红黑树

- LinkedHashMap，底层为HashMap + 双向链表，其中有一个属性accessOrder，默认false表示按照插入的顺序访问；当设置为true时，当元素被访问时，会被移动到链表的末位。当添加元素时，会调用removeEldestEntry()判断是否要删除头部的元素（默认是不删除的）

  可以用LinkedHashMap来做LRU（最近最少使用），在构造中accessOrder传true，并重写removeEldestEntry()方法即可。

- TreeMap：底层是红黑树

- 红黑树的平衡：插入、删除。左旋、右旋、变色

