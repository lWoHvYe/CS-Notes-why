## Java-容器

### 集合

- Set（集）、List（列表）、Map（映射）

- 数组、链表、二叉树（二叉搜索树、红黑树）、堆、栈、图

- 先通过HashCode判断，相同再用equals判断，为何重写equals要重写HashCode方法

- ArrayList扩容为1.5倍。int newCapacity = oldCapacity + (oldCapacity >> 1)

- HashMap

    - 1.8之前：数组 + 链表
    - 1.8开始：数组 + 链表 + 红黑树
    - 容量始终保持为2^n，扩容时增大一倍，初始及扩容的源码（判断容量是否达到最大、重新计算index，针对rehash进行了优化）
    - 1.8针对rehash的优化：由于数组的容量是以2的幂次方扩容的，那么一个Entity在扩容时，新的位置要么在原位置，要么在原长度+原位置的位置。数组长度变为原来的2倍，表现在二进制上就是多了一个高位参与数组下标确定。
      此时，一个元素通过hash转换坐标的方法计算后，恰好出现一个现象：参与运算的最高位是0则坐标不变，最高位是1则坐标变为“原长度+原坐标”。
  ``` java
  if ((e.hash & oldCap) == 0) {
     还放在index桶
  } else {
     放在index + oldCap新桶中
  }
  ```
    - 容量保持2^n，是为了使用这个位运算的性质：若x为2的n次幂，则 y%x 等同于 y&(x-1)，位运算的速度要高于取模运算
    - 1.8之前，扩容时链表会反转，从而导致在并发环境下可能死循环。1.8开始不再反转，并把头插法改为尾插法

- ConcurrentHashMap

    - 1.8之前：Segment段 （ReentrantLock加锁），段数（并行度）默认16，初始化后不会改变

      结构：Segment 数组 + HashEntry 数组 + 链表

    - 1.8开始：使用的 Synchronized 锁加 CAS 的机制

      结构：Node 数组 + 链表 / 红黑树

- LinkedHashMap，底层为HashMap +
  双向链表，其中有一个属性accessOrder，默认false表示按照插入的顺序访问；当设置为true时，当元素被访问时，会被移动到链表的末位。当添加元素时，会调用removeEldestEntry()
  判断是否要删除头部的元素（默认是不删除的）

  可以用LinkedHashMap来做LRU（最近最少使用），在构造中accessOrder传true，并重写removeEldestEntry()方法即可。

- TreeMap：底层是红黑树

- 红黑树的平衡：插入、删除。左旋、右旋、变色

