# 一、java面试题
## 1. HashMap实现原理（难度星数：★★★★）

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

## 2.ConcurrentHashMap实现原理（难度星数：★★★★）
待补充......

## 3.ArrayList实现原理、扩容机制（难度星数：★★★）
待补充......

## 4.LinkedList实现原理、扩容机制（难度星数：★★★）
待补充......

## 5.ArrayList和LinkedList的区别?（难度星数：★★）
最明显的区别是 ArrrayList底层的数据结构是数组，支持随机访问，而 LinkedList 的底层数据结构是双向循环链表，不支持随机访问。使用下标访问一个元素，ArrayList 的时间复杂度是 O(1)，而 LinkedList 是 O(n)。

待补充......


## 36.讲讲java中的反射？

## 37.反射中，Class.forName和classloader的区别（难度星数：★★）
java中class.forName()和classLoader都可用来对类进行加载。  
- class.forName()前者除了将类的.class文件加载到jvm中之外，还会对类进行解释，执行类中的static块。  
- 而classLoader只干一件事情，就是将.class文件加载到jvm中，不会执行static中的内容,只有在newInstance才会去执行static块。    
- Class.forName(name, initialize, loader)带参函数也可控制是否加载static块。并且只有调用了newInstance()方法采用调用构造函数，创建类的对象。  

## 38.java内部类有哪些

## 39.Java中父类与子类静态代码块、非静态代码块、构造函数的加载顺序（难度星数：★★）

父类静态代码块 -> 子类静态代码块 -> 父类构造代码块 -> 父类构造函数 -> 子类构造代码块 -> 子类构造函数
静态代码块,在这个类加载的时候会执行,并且只执行一次,在创建对象之前会执行非静态代码块,并且在每次创建对象之前都会执行一次。  

总之：静态代码块总是最先执行的; 
子类和父类的静态代码块都执行完之后，在执行父类的非静态代码块和父类的构造方法,最后执行子类的非静态代码块和构造方法.

## 40.Comparator和Comparable的区别?（难度星数：★）
Comparable 接口用于定义对象的自然顺序，而 comparator 通常用于定义用户定制的顺序。
Comparable 总是只有一个，但是可以有多个 comparator 来定义对象的顺序。

## 41.Java的数据结构有哪些？（难度星数：★）
- 线性表（ArrayList）  
- 链表（LinkedList）  
- 栈（Stack）  
- 队列（Queue）  
- 图（Map）  
- 树（Tree）  

## 42.java 创建对象的几种方式（难度星数：★）
1.采用new  
2.通过反射  
3.采用clone  
4.通过序列化机制  

## 43.java当中的四种引用（难度星数：★★）
强引用，软引用，弱引用，虚引用。不同的引用类型主要体现在GC上:  

1.强引用：如果一个对象具有强引用，它就不会被垃圾回收器回收。即使当前内存空间不足，JVM也不会回收它，而是抛出 OutOfMemoryError 错误，使程序异常终止。如果想中断强引用和某个对象之间的关联，可以显式地将引用赋值为null，这样一来的话，JVM在合适的时间就会回收该对象。

2.软引用：在使用软引用时，如果内存的空间足够，软引用就能继续被使用，而不会被垃圾回收器回收，只有在内存不足时，软引用才会被垃圾回收器回收。

3.弱引用：具有弱引用的对象拥有的生命周期更短暂。因为当 JVM 进行垃圾回收，一旦发现弱引用对象，无论当前内存空间是否充足，都会将弱引用回收。不过由于垃圾回收器是一个优先级较低的线程，所以并不一定能迅速发现弱引用对象。

4.虚引用：顾名思义，就是形同虚设，如果一个对象仅持有虚引用，那么它相当于没有引用，在任何时候都可能被垃圾回收器回收。


## 44.使用final关键字修饰一个变量时，是引用不能变，还是引用的对象不能变？（难度星数：★）
使用final关键字修饰一个变量时，是指引用变量不能变，引用变量所指向的对象中的内容还是可以改变的。例如，对于如下语句：
```java
final StringBuffer a = new StringBuffer("immutable");
```

执行如下语句将报告编译期错误：
```java
A = new StringBuffer("");
```

但是，执行如下语句则可以通过编译：
```java
a.append(" broken!");
```

## 45.列出一些你常见的运行时异常？（难度星数：★）
1.ArithmeticException（算术异常）;  
2.ClassCastException （类转换异常）;  
3.IllegalArgumentException （非法参数异常）;  
4.ndexOutOfBoundsException （下标越界异常）;  
5.NullPointerException （空指针异常）;  
6.SecurityException （安全异常）  

## 46.Java线程具有五种状态（难度星数：★）

**1. 新建状态（New）**  
当线程对象对创建后，即进入了新建状态，如：Thread t = new MyThread()。

**2. 就绪状态（Runnable）**  
当调用线程对象的 start()方法（t.start();），线程 即进入就绪状态。处于就绪状态的线程，只是说明此线程已经做好了准备，随时 等待 CPU 调度执行，并不是说执行了 t.start()此线程立即就会执行； 

**3. 运行状态（Running）**  
当 CPU 开始调度处于就绪状态的线程时，此时线程 才得以真正执行，即进入到运行状态。注：就 绪状态是进入到运行状态的唯一入 口，也就是说，线程要想进入运行状态执行，首先必须处于就绪状态中； 

**4. 阻塞状态（Blocked）**  
处于运行状态中的线程由于某种原因，暂时放弃对 CPU 的使用权，停止执行，此时进入阻塞状态，直到其进入到就绪状态，才 有机会再 次被 CPU 调用以进入到运行状态。 根据阻塞产生的原因不同，
阻塞状态又可以分为三种： 
 1、等待阻塞：运行状态中的线程执行 wait()方法，使本线程进入到等待阻塞状态； 
 2、同步阻塞：线程在获取 synchronized 同步锁失败(因为锁被其它线程所占用)， 它会进入同步阻塞状态； 
 3、其他阻塞：通过调用线程的 sleep()或 join()或发出了 I/O 请求时，线程会进入 到阻塞状态。当 sleep()状态超时、join()等待线程终止或者超时、或者 I/O 处理 完毕时，线程重新转入就绪状态。 

**5. 死亡状态（Dead）**  
线程执行完了或者因异常退出了 run()方法，该线程结束 生命周期。
