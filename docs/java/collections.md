## Java的数据结构有哪些？

- 线性表（ArrayList）  
- 链表（LinkedList）  
- 栈（Stack）  
- 队列（Queue）  
- 图（Map）  
- 树（Tree）  

## HashMap实现原理

HashMap实际上是一个“链表散列”的数据结构，即数组和链表的结合体。
HashMap底层就是一个数组结构，数组中的每一项又是一个链表。当新建一个HashMap的时候，就会初始化一个数组。

```java
/**
* The table, resized as necessary. Length MUST Always be a power of two.
 */
transient Entry[] table;
 
static class Node<K,V> implements Map.Entry<K,V> {
    final K key;
    V value;
    Entry<K,V> next;
    final int hash;
    ......
}
```

当我们往HashMap中put元素的时候，先根据key的hashCode重新计算hash值，根据hash值得到这个元素在数组中的位置（即下标）， 如果数组该位置上已经存放有其他元素了，那么在这个位置上的元素将以链表的形式存放，新加入的放在链头，最先加入的放在链尾。如果数组该位置上没有元素，就直接将该元素放到此数组中的该位置上。
简单地说，HashMap 在底层将 key-value 当成一个整体进行处理，这个整体就是一个 Entry 对象。HashMap 底层采用一个 Entry[] 数组来保存所有的 key-value 对，当需要存储一个 Entry 对象时，会根据hash算法来决定其在数组中的存储位置，再根据equals方法决定其在该数组位置上的链表中的存储位置；当需要取出一个Entry时，
也会根据hash算法找到其在数组中的存储位置，再根据equals方法从该位置上的链表中取出该Entry。

HashMap的性能参数：
HashMap 包含如下几个构造器：

1．HashMap()：构建一个初始容量为 16，负载因子为 0.75 的 HashMap。  
2．HashMap(int initialCapacity)：构建一个初始容量为 initialCapacity，负载因子为 0.75 的 HashMap。  
3．HashMap(int initialCapacity, float loadFactor)：以指定初始容量、指定的负载因子创建一个 HashMap。  
4．HashMap的基础构造器HashMap(int initialCapacity, float loadFactor)带有两个参数，它们是初始容量initialCapacity和负载因子loadFactor。  

负载因子loadFactor衡量的是一个散列表的空间的使用程度，负载因子越大表示散列表的装填程度越高，反之愈小。对于使用链表法的散列表来说，查找一个元素的平均时间是O(1+a)，因此如果负载因子越大，对空间的利用更充分，然而后果是查找效率的降低；如果负载因子太小，那么散列表的数据将过于稀疏，对空间造成严重浪费。

HashMap操作的时间复杂度：
理想情况下HashMap的时间复杂度为O（1）

## ConcurrentHashMap实现原理

JDK1.7版本： 容器中有多把锁，每一把锁锁一段数据，这样在多线程访问时不同段的数据时，就不会存在锁竞争了，这 样便可以有效地提高并发效率。这就是ConcurrentHashMap所采用的"分段锁"思想

每一个segment都是一个HashEntry<K,V>[] table， table中的每一个元素本质上都是一个HashEntry的单向队列（原理和hashMap一样）。比如table[3]为首节点，table[3]->next为节点1，之后为节点2，依次类推。

JDK1.8版本：做了2点：

取消segments字段，直接采用transient volatile HashEntry<K,V>[] table保存数据，采用table数组元素作为锁，从而实现了对每一行数据进行加锁，并发控制使用Synchronized和CAS来操作
将原先table数组＋单向链表的数据结构，变更为table数组＋单向链表＋红黑树的结构

在ConcurrentHashMap中通过一个Node<K,V>[]数组来保存添加到map中的键值对，而在同一个数组位置是通过链表和红黑树的形式来保存的。但是这个数组只有在第一次添加元素的时候才会初始化，否则只是初始化一个ConcurrentHashMap对象的话，只是设定了一个sizeCtl变量，这个变量用来判断对象的一些状态和是否需要扩容

第一次添加元素的时候，默认初期长度为16，当往map中继续添加元素的时候，通过hash值跟数组长度取与来决定放在数组的哪个位置，如果出现放在同一个位置的时候，优先以链表的形式存放，在同一个位置的个数又达到了8个以上，如果数组的长度还小于64的时候，则会扩容数组。如果数组的长度大于等于64了的话，在会将该节点的链表转换成树。

通过扩容数组的方式来把这些节点给分散开。然后将这些元素复制到扩容后的新的数组中，同一个链表中的元素通过hash值的数组长度位来区分，是还是放在原来的位置还是放到扩容的长度的相同位置去 。在扩容完成之后，如果某个节点的是树，同时现在该节点的个数又小于等于6个了，则会将该树转为链表。

取元素的时候，相对来说比较简单，通过计算hash来确定该元素在数组的哪个位置，然后在通过遍历链表或树来判断key和key的hash，取出value值。

## ArrayList实现原理

## LinkedList实现原理

## ArrayList和LinkedList的区别?

1.是否保证线程安全：ArrayList 和 LinkedList 都是不同步的，也就是不保证线程安全；     
 
2.底层数据结构： Arraylist 底层使用的是 Object 数组；LinkedList 底层使用的是 双向链表 数据结构（JDK1.6 之前为循环链表，JDK1.7 取消了循环。注意双向链表和双向循环链表的区别，下面有介绍到！）    

3.插入和删除是否受元素位置的影响： ① ArrayList 采用数组存储，所以插入和删除元素的时间复杂度受元素位置的影响。 比如：执行add(E e)方法的时候， 
ArrayList 会默认在将指定的元素追加到此列表的末尾，这种情况时间复杂度就是 O(1)。但是如果要在指定位置 i 插入和删除元素的话（add(int index, E element)）
时间复杂度就为 O(n-i)。因为在进行上述操作的时候集合中第 i 和第 i 个元素之后的(n-i)个元素都要执行向后位/向前移一位的操作。 ② LinkedList 采用链表存储，所以对于add(E e)方法的插入，
删除元素时间复杂度不受元素位置的影响，近似 O(1)，如果是要在指定位置i插入和删除元素的话（(add(int index, E element)） 时间复杂度近似为o(n))因为需要先移动到指定位置再插入。    

4.是否支持快速随机访问： LinkedList 不支持高效的随机元素访问，而 ArrayList 支持。快速随机访问就是通过元素的序号快速获取元素对象(对应于get(int index)方法)。    

5.内存空间占用： ArrayList 的空 间浪费主要体现在在 list 列表的结尾会预留一定的容量空间，而 LinkedList 的空间花费则体现在它的每一个元素都需要消耗比 ArrayList 更多的空间（因为要存放直接后继和直接前驱以及数据）。  
