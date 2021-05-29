- [线程基础](#线程基础理解)
  - [进程和线程的区别](#进程和线程的区别)
  - [什么是上下文切换](#什么是上下文切换)
  - [什么是线程死锁](#什么是线程死锁)
  - [如何预防线程死锁](#如何预防线程死锁)
  - [线程的创建方式](#线程的创建方式)
  - [Java线程具有五种状态](#Java线程具有五种状态)
  - [Java中sleep和wait的区别](#Java中sleep和wait的区别)
  - [java中的synchronized原理](#synchronized实现原理)  
  - [java中的volatile理解](#volatile理解)
  - [synchronized和volatile的区别](#synchronized和volatile的区别)
  - [java中的CAS操作](#java中的CAS操作)
  - [java指令重排序](#java指令重排序)
  - [锁的概述](#锁的概述)  
    - [乐观锁和悲观锁区别](#乐观锁和悲观锁区别)
    - [什么是可重入锁](#什么是可重入锁)  
    - [什么是自旋锁](#什么是自旋锁)  
    - [公平锁和非公平锁的区别](#公平锁和非公平锁的区别)
    - [独占锁和共享锁的区别](#独占锁和共享锁的区别) 
- [JUC包下常用类](#JUC包下常用类)     
  - [ThreadLocalRandom](#ThreadLocalRandom)
  - [LongAdder](#LongAdder)
  - [LockSupport工具类](#LockSupport工具类)
  - [AQS原理](#AQS原理)
  - [ReentrantLock实现原理](#ReentrantLock实现原理)
  - [synchronized和ReentrantLock的区别](#synchronized和ReentrantLock的区别)
  - [读写锁ReentrantReadWriteLock的原理](#ReentrantReadWriteLock原理)
  - [StampedLock锁](#StampedLock锁)
  - [ThreadLocal实现原理和内存泄露](#ThreadLocal实现原理和内存泄露)
  - [ConcurrentHashMap分析](#ConcurrentHashMap分析)
- [ThreadPoolExecutor线程池](#ThreadPoolExecutor线程池)  
  - [ThreadPoolExecutor详解](#ThreadPoolExecutor详解)
- [Java并发包中线程同步器](#Java并发包中线程同步器)  
  - [CountDownLatch原理和使用](#CountDownLatch原理和使用)
  - [CyclicBarrier原理和使用](#CyclicBarrier原理和使用)
  - [Semaphore原理和使用](#Semaphore原理和使用)
- [Java并发包中的阻塞队列](#阻塞队列)  
  - [ConcurrentLinkedQueue原理](#ConcurrentLinkedQueue原理)
  - [LinkedBlockingQueue原理](#LinkedBlockingQueue原理)
  - [ArrayBlockingQueue原理](#ArrayBlockingQueue原理)
  - [PriorityBlockingQueue原理](#PriorityBlockingQueue原理)
  - [DelayQueue原理](#DelayQueue原理)
  - [SynchronousQueue](#SynchronousQueue)
  - [LinkedTransferQueue](#LinkedTransferQueue)


## 线程基础理解

### 进程和线程的区别
- 进程是代码在数据集合上的一次运行活动，是系统进行资源分配和调度的基本单元。
- 线程则是进程的执行路径，一个进程至少有一个线程，线程是CPU分配的基本单元。

### 什么是上下文切换?

多线程编程中一般线程的个数都大于 CPU 核心的个数，而一个 CPU 核心在任意时刻只能被一个线程使用，为了让这些线程都能得到有效执行，CPU 采取的策略是为每个线程分配时间片并轮转的形式。当一个线程的时间片用完的时候就会重新处于就绪状态让给其他线程使用，这个过程就属于一次上下文切换。

概括来说就是：当前任务在执行完 CPU 时间片切换到另一个任务之前会先保存自己的状态，以便下次再切换回这个任务时，可以再加载这个任务的状态。任务从保存到再加载的过程就是一次上下文切换。

上下文切换通常是计算密集型的。也就是说，它需要相当可观的处理器时间，在每秒几十上百次的切换中，每次切换都需要纳秒量级的时间。所以，上下文切换对系统来说意味着消耗大量的 CPU 时间，事实上，可能是操作系统中时间消耗最大的操作。

Linux 相比与其他操作系统（包括其他类 Unix 系统）有很多的优点，其中有一项就是，其上下文切换和模式切换的时间消耗非常少。

### 什么是线程死锁？

多个线程同时被阻塞，它们中的一个或者全部都在等待某个资源被释放。由于线程被无限期地阻塞，因此程序不可能正常终止。

产生死锁必须具备以下四个条件：

1. 互斥条件：该资源任意一个时刻只由一个线程占用。
1. 请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放。
1. 不剥夺条件:线程已获得的资源在末使用完之前不能被其他线程强行剥夺，只有自己使用完毕后才释放资源。
1. 循环等待条件:若干进程之间形成一种头尾相接的循环等待资源关系。

### 如何预防线程死锁?

我们只要破坏产生死锁的四个条件中的其中一个就可以了。

**破坏互斥条件**

这个条件我们没有办法破坏，因为我们用锁本来就是想让他们互斥的（临界资源需要互斥访问）。

**破坏请求与保持条件**

一次性申请所有的资源。

**破坏不剥夺条件**

占用部分资源的线程进一步申请其他资源时，如果申请不到，可以主动释放它占有的资源。

**破坏循环等待条件**

靠按序申请资源来预防。按某一顺序申请资源，释放资源则反序释放。破坏循环等待条件。

### 线程的创建方式
- 继承Thread类
- 实现runnable接口
- 实现callable接口，有返回值

### Java线程具有五种状态

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

### Java中sleep和wait的区别

- sleep方法是Thread类的静态方法，wait方法是Object类的成员方法。  
- sleep方法不会释放lock，但是wait会释放lock，而且会加入等待队列中  
- sleep方法不依赖于同步器synchronized，但是wait需要依赖synchronized关键字  
- sleep方法有可能会抛出异常，所以需要进行异常处理；wait方法不需要处理  
- sleep方法可以在任何地方使用；wait方法只能在同步方法和同步代码块中使用  

### synchronized实现原理

**1. 作用**  
原子性：确保线程互斥的访问同步代码；
可见性：保证共享变量的修改能够及时可见，其实是通过Java内存模型中的 “对一个变量unlock操作之前，必须要同步到主内存中；如果对一个变量进行lock操作，则将会清空工作内存中此变量的值，在执行引擎使用此变量前，需要重新从主内存中load操作或assign操作初始化变量值” 来保证的；
有序性：有效解决重排序问题，即 “一个unlock操作先行发生(happen-before)于后面对同一个锁的lock操作”；

**2. 三种用法** 
当synchronized作用在实例方法时，监视器锁（monitor）便是对象实例（this）；
当synchronized作用在静态方法时，监视器锁（monitor）便是对象的Class实例，因为Class数据存在于永久代，因此静态方法锁相当于该类的一个全局锁；
当synchronized作用在某一个对象实例时，监视器锁（monitor）便是括号括起来的对象实例；

**3. 实现原理**  
 
修饰代码块：

monitorenter：每个对象都是一个监视器锁（monitor）。当monitor被占用时就会处于锁定状态，线程执行monitorenter指令时尝试获取monitor的所有权，过程如下：

修饰方法：

Synchronized方法同步不再是通过插入monitorentry和monitorexit指令实现，而是由方法调用指令来读取运行时常量池中的ACC_SYNCHRONIZED标志隐式实现的，如果方法表结构（method_info Structure）中的ACC_SYNCHRONIZED标志被设置，
那么线程在执行方法前会先去获取对象的monitor对象，如果获取成功则执行方法代码，执行完毕后释放monitor对象，如果monitor对象已经被其它线程获取，那么当前线程被阻塞。

**4. 锁升级过程** 

锁的4中状态：无锁状态、偏向锁状态、轻量级锁状态、重量级锁状态（级别从低到高）

1）偏向锁：

为什么要引入偏向锁？

因为经过HotSpot的作者大量的研究发现，大多数时候是不存在锁竞争的，常常是一个线程多次获得同一个锁，因此如果每次都要竞争锁会增大很多没有必要付出的代价，为了降低获取锁的代价，才引入的偏向锁。

偏向锁的升级

当线程1访问代码块并获取锁对象时，会在java对象头和栈帧中记录偏向的锁的threadID，因为偏向锁不会主动释放锁，因此以后线程1再次获取锁的时候，需要比较当前线程的threadID和Java对象头中的threadID是否一致，如果一致（还是线程1获取锁对象），则无需使用CAS来加锁、解锁；如果不一致（其他线程，如线程2要竞争锁对象，而偏向锁不会主动释放因此还是存储的线程1的threadID），那么需要查看Java对象头中记录的线程1是否存活，如果没有存活，那么锁对象被重置为无锁状态，其它线程（线程2）可以竞争将其设置为偏向锁；如果存活，那么立刻查找该线程（线程1）的栈帧信息，如果还是需要继续持有这个锁对象，那么暂停当前线程1，撤销偏向锁，升级为轻量级锁，如果线程1 不再使用该锁对象，那么将锁对象状态设为无锁状态，重新偏向新的线程。

偏向锁的取消：

偏向锁是默认开启的，而且开始时间一般是比应用程序启动慢几秒，如果不想有这个延迟，那么可以使用-XX:BiasedLockingStartUpDelay=0；

如果不想要偏向锁，那么可以通过-XX:-UseBiasedLocking = false来设置；

（2）轻量级锁

为什么要引入轻量级锁？

轻量级锁考虑的是竞争锁对象的线程不多，而且线程持有锁的时间也不长的情景。因为阻塞线程需要CPU从用户态转到内核态，代价较大，如果刚刚阻塞不久这个锁就被释放了，那这个代价就有点得不偿失了，因此这个时候就干脆不阻塞这个线程，让它自旋这等待锁释放。

轻量级锁什么时候升级为重量级锁？

线程1获取轻量级锁时会先把锁对象的对象头MarkWord复制一份到线程1的栈帧中创建的用于存储锁记录的空间（称为DisplacedMarkWord），然后使用CAS把对象头中的内容替换为线程1存储的锁记录（DisplacedMarkWord）的地址；

如果在线程1复制对象头的同时（在线程1CAS之前），线程2也准备获取锁，复制了对象头到线程2的锁记录空间中，但是在线程2CAS的时候，发现线程1已经把对象头换了，线程2的CAS失败，那么线程2就尝试使用自旋锁来等待线程1释放锁。

但是如果自旋的时间太长也不行，因为自旋是要消耗CPU的，因此自旋的次数是有限制的，比如10次或者100次，如果自旋次数到了线程1还没有释放锁，或者线程1还在执行，线程2还在自旋等待，这时又有一个线程3过来竞争这个锁对象，那么这个时候轻量级锁就会膨胀为重量级锁。重量级锁把除了拥有锁的线程都阻塞，防止CPU空转。


## volatile理解

1）保证内存可见性

通俗来说就是，线程A对一个volatile变量的修改，对于其它线程来说是可见的，即线程每次获取volatile变量的值都是最新的。

当一个变量被 volatile 修饰时，任何线程对它的写操作都会立即刷新到主内存中，并且会强制让缓存了该变量的线程中的数据清空，必须从主内存重新读取最新数据。

2）禁止指令重排序
禁止指令重排序的原因：
JMM是允许编译器和处理器对指令重排序的，但是规定了as-if-serial语义，程序的执行结果不能改变

说明：volatile并不能保证原子性

### synchronized和volatile的区别？

一旦一个共享变量（类的成员变量、类的静态成员变量）被volatile修饰之后，那么就具备了两层语义：   
1.保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的。  
2.禁止进行指令重排序。    
   volatile本质是在告诉jvm当前变量在寄存器（工作内存）中的值是不确定的，需要从主存中读取；  
   synchronized则是锁定当前变量，只有当前线程可以访问该变量，其他线程被阻塞住。  

- volatile仅能使用在变量级别；synchronized则可以使用在变量、方法、和类级别的。  
- volatile仅能实现变量的修改可见性，并不能保证原子性；synchronized则可以保证变量的修改可见性和原子性。  
- volatile不会造成线程的阻塞；synchronized可能会造成线程的阻塞。  
- volatile标记的变量不会被编译器优化；synchronized标记的变量可以被编译器优化。  

### synchronized和ReentrantLock的区别

synchronized是和if、else、for、while一样的关键字，ReentrantLock是类，这是二者的本质区别。既然ReentrantLock是类，那么它就提供了比synchronized更多更灵活的特性，可以被继承、可以有方法、可以有各种各样的类变量，ReentrantLock比synchronized的扩展性体现在几点上： 

- ReentrantLock可以对获取锁的等待时间进行设置，这样就避免了死锁 
- ReentrantLock可以获取各种锁的信息 
- ReentrantLock可以灵活地实现多路通知 
- 另外，二者的锁机制其实也是不一样的:ReentrantLock底层调用的是Unsafe的park方法加锁，synchronized操作的应该是对象头中mark word.

### java中的CAS操作

### java指令重排序


## 锁的概述

### 乐观锁和悲观锁区别

- 乐观锁：乐观锁认为竞争不总是会发生，因此它不需要持有锁，将比较-替换这两个动作作为一个原子操作尝试去修改内存中的变量，如果失败则表示发生冲突，那么就应该有相应的重试逻辑。
- 悲观锁：悲观锁认为竞争总是会发生，因此每次对某资源进行操作时，都会持有一个独占的锁，就像synchronized，不管三七二十一，直接上了锁就操作资源了。

### 什么是可重入锁？

一个线程已经获取锁时再次获取锁不会对其自身进行阻塞的锁叫做可重入锁。

##% 什么是自旋锁？

自旋锁（spinlock）：是指当一个线程在获取锁的时候，如果锁已经被其它线程获取，那么该线程将循环等待，然后不断的判断锁是否能够被成功获取，直到获取到锁才会退出循环。

自旋锁是使用CPU时间缓存线程阻塞与调度的开销。

### 公平锁和非公平锁的区别

- 公平锁：多个线程按照申请锁的顺序去获得锁，线程会直接进入队列去排队，永远都是队列的第一位才能得到锁。

优点：所有的线程都能得到资源，不会饿死在队列中。
缺点：吞吐量会下降很多，队列里面除了第一个线程，其他的线程都会阻塞，cpu唤醒阻塞线程的开销会很大。

- 非公平锁：多个线程去获取锁的时候，会直接去尝试获取，获取不到，再去进入等待队列，如果能获取到，就直接获取到锁。

优点：可以减少CPU唤醒线程的开销，整体的吞吐效率会高点，CPU也不必取唤醒所有线程，会减少唤起线程的数量。
缺点：你们可能也发现了，这样可能导致队列中间的线程一直获取不到锁或者长时间获取不到锁，导致饿死

### 独占锁和共享锁的区别

- 独占锁也叫排他锁，是指该锁一次只能被一个线程所持有。如果线程T对数据A加上排他锁后，则其他线程不能再对A加任何类型的锁。获得排它锁的线程即能读数据又能修改数据。JDK中的synchronized和 JUC中Lock的实现类就是互斥锁。

- 共享锁是指该锁可被多个线程所持有。如果线程T对数据A加上共享锁后，则其他线程只能对A再加共享锁，不能加排它锁。获得共享锁的线程只能读数据，不能修改数据。 独享锁与共享锁也是通过AQS来实现的，通过实现不同的方法，来实现独享或者共享。

## JUC包下常用类

### ThreadLocalRandom

### LongAdder

### AQS原理

### ReentrantLock实现原理

### synchronized和ReentrantLock的区别

### ReentrantLock实现原理

ReentrantLock是基于AQS实现可重入的独占锁，同时只能有一个线程可以获取该锁，其他获取该锁的线程会被阻塞而放入该锁的AQS阻塞队列里面。根据参数来决定其内部是一个公平锁还是
非公平锁，默认是非公平锁。ReentrantLock里面的Sync类直接继承AQS，它的子类NonfairSync和FairSync分别实现了获取锁的非公平和公平策略。

在这里，AQS的state状态值表示线程获取该锁的可重入次数，在默认情况下，state的值为0表示当前锁没有被任何线程持有。当一个线程第一次获取该锁时会尝试使用CAS设置state的值为1，如果CAS成功则当前线程获取了该锁，
然后记录改锁的持有者为当前线程。在该线程没有释放锁的情况下第二次获取该锁后，状态值被设置为2，这就是可重入次数。在该线程释放该锁时，会尝试使用CAS让状态值减1，如果减1后状态值为0，则当前线程释放该锁。

### ReentrantReadWriteLock的原理

### StampedLock锁

### ThreadLocal实现原理和内存泄露

### ConcurrentHashMap分析

### ThreadPoolExecutor线程池


## Java并发包中线程同步器

### CountDownLatch原理

**1. 原理**  
CountDownLatch是使用AQS实现的，使用AQS的状态变量来存放计数器的值。首先在初始化CountDownLatch时设置状态值（计算器值），当多个线程调用countdown
方法时实际是原子性递减AQS的状态值。当线程调用await方法后当前线程会被放入AQS的阻塞队列等待计数器为0再返回。其他线程调用countdown方法让计数器值递减1，
当计数器值变为0时，当前线程还要调用AQS的doReleaseShared方法来激活由于调用await()方法而被阻塞的线程。

**2. 使用**

### CyclicBarrier原理

CyclicBarrier是回环屏障的意思,它可以让一组线程全部达到一个状态后再全部同时执行。这里之所以叫作回环是因为当所有等待线程执行完毕，并重置CyclicBarrier 的状态后它可以被重用。之所以叫作屏障是因为线程调用await 方法后就会被阻塞，这个阻塞点就称为屏障点，等所有线程都调用了await方法后，线程们就会冲破屏障，继续向下运行。

**1. 原理**

CyclicBarrier基于独占锁实现,本质底层还是基于AQS的。parties用来记录线程个数，这里表示多少线程调用await后，所有线程才会冲破屏障继续往下运行。而count一开始等于parties,每当有线程调用await方法就递减1,当count为0时就表示所有线程都到了屏障点。

你可能会疑惑，为何维护parties和count两个变量，只使用count不就可以了?另外别忘了CyclieBarrier是可以被复用的，使用两个变量的原因是,parties始终用来记录总的线程个数，当count计数器值变为0后，会将parties的值赋给count,从而进行复用。

还有一个变量barrierCommand也通过构造函数传递，这是一个任务，这个任务的执行时机是当所有线程都到达屏障点后。使用lock首先保证了更新计数器count的原子性。另外使用lock 的条件变量trip支持线程间使用await和signal操作进行同步。

最后，在变量generation内部有一个变量broken，其用来记录当前屏障是否被打破。注意,这里的broken并没有被声明为volatile的,因为是在锁内使用变量,所以不需要声明。

### 信号量Semaphore原理

Semaphore信号量也是Java中的一个同步器，与CountDownLatch和CycleBarrier不同的是它内部的计数器是递增的，并且一开始初始化Semaphore时可以指定一个初始值，但是并不需要知道需要同步的线程个数，而是在需要同步的地方调用acquire方法时指定需要同步的线程个数

**1. 原理**

Semaphore还是使用AQS来实现的。Sync只是对AQS的一个修饰，Sync有两个实现类，用来指定获取信号量时采用的公平策略。

```java
   public Semaphore(int permits) {
        sync = new NonfairSync(permits);
    }

    public Semaphore(int permits, boolean fair) {
        sync = fair ? new FairSync(permits) : new NonfairSync(permits);
    }
```
如上代码中Semaphore默认采用非公平策略。并且Semaphore的信号量的个数也是通过AQS中的state的值来实现的。

- void acquire()

调用该方法是希望获取一个信号量资源。如果当前信号量个数大于0，则当前信号量个数会减1，然后该方法直接返回。如果当前信号量个数等于0，则当前线程会被放入AQS的阻塞队列中。如果其它线程调用了该线程的interrupt方法时，则会中断该线程然后抛出异常返回。

```java
    public void acquire() throws InterruptedException {
    	//传递参数为1，说明要获取一个信号量资源
        sync.acquireSharedInterruptibly(1);
    }
    public final void acquireSharedInterruptibly(int arg)
            throws InterruptedException {
            //如果线程被中断则抛出异常
        if (Thread.interrupted())
            throw new InterruptedException();
            //否则调用Sync子类尝试获取。
        if (tryAcquireShared(arg) < 0)
        	//如果调用失败则放入阻塞队列，然后再次尝试
            doAcquireSharedInterruptibly(arg);
    }
```
在以上代码中acquire在内部调用Sync的acquireSharedlnterruptibly方法，后者对中断进行相应。尝试获取信号量资源的AQS的方法tryAcquireShared是由Sync的子类实现的，所以这里分别从两方面来讨论。

```java
  final int nonfairTryAcquireShared(int acquires) {
       for (;;) {
       		//获取当前信号量值
           int available = getState();
           //计算当前剩余值
           int remaining = available - acquires;
           //如果当前剩余值小于0或者CAS设置成功返回
           if (remaining < 0 ||
               compareAndSetState(available, remaining))
               return remaining;
       }
   }
```

如上代码中先获取当前信号量值（available），然后减去需要获取的值（acquires），得到剩余的信号量个数（remaining），如果剩余值小于0则说明当前信号量个数满足不了需求，那么直接返回负数，这时当前线程会被放入AQS的阻塞队列而被挂起。如果剩余值大于0，则使用CAS操作设置当前信号量为剩余值，然后返回剩余值。
以上是非公平的逻辑，公平逻辑如下：

```java
 protected int tryAcquireShared(int acquires) {
     for (;;) {
         if (hasQueuedPredecessors())
             return -1;
         int available = getState();
         int remaining = available - acquires;
         if (remaining < 0 ||
             compareAndSetState(available, remaining))
             return remaining;
     }
 }
```
在公平策略中会通过hasQueuedPredecessors函数去AQS队列中查询是否有头结点的线程，如果有则优先获取。

```java
- void acquire(int permits)

    public void acquire(int permits) throws InterruptedException {
        if (permits < 0) throw new IllegalArgumentException();
        sync.acquireSharedInterruptibly(permits);
    }
```
该方法中与acquire方法不同，acquire只需要获取一个信号量，acquire(int permits)需要获取permits个。

- void acquireUninterruptibly()

该方法与aquire () 似，不同之处在于该方法 对中断不响应，也就是当当前线程调用了 acquireUninterruptibly 获取资源时（包含被阻塞后 ），其他线程调用了当前线程的 interrupt ( ）方法设置了当前线程的中断标志，此时当前线程并不会抛出
IntrruptException 异常而返回。
void release()

该方法的作用是把当前Semaphore对象的信号量加1，如果当前有线程调用acquire方法而被阻塞放入AQS阻塞队列中，则会根据公平策略选择一个信号量个数能被满足的线程进行激活，激活的线程会尝试获取刚增加的信号量。

```java
    public void release() {
        sync.releaseShared(1);
    }
    public final boolean releaseShared(int arg) {
    	//尝试释放资源
        if (tryReleaseShared(arg)) {
        	//资源释放成功则调用park方法唤醒AQS中先挂起的线程
            doReleaseShared();
            return true;
        }
        return false;
    }
        protected final boolean tryReleaseShared(int releases) {
            for (;;) {
            	//获取当前信号量
                int current = getState();
                //将当前信号量增加release
                int next = current + releases;
                if (next < current) // overflow
                    throw new Error("Maximum permit count exceeded");
                    //使用CAS保证更新信号量值的原子性
                if (compareAndSetState(current, next))
                    return true;
            }
        }
```
release()->sync.releaseShared(1)可知，release方法每次只对信号量增加1，tryReleaseShared方法是无限循环，使用CAS保证release方法对信号量递增1的原子性操作。tryReleaseShared方法增加信号量值成功后会调用AQS的方法来激活因为调用acquire方法而被阻塞的线程。

**总结：**

Semaphore的计数器是不可以自动重置的，不过通过变相的改变acquire方法的参数可以实现CycleBarrier的功能。Semaphore也是通过AQS实现的，并且获取信号量是分为公平和非公平策略

## Java并发包中的阻塞队列

### ConcurrentLinkedQueue原理

待补充......

### LinkedBlockingQueue原理

待补充......

### ArrayBlockingQueue原理

待补充......

### PriorityBlockingQueue原理

待补充......

### DelayQueue原理

待补充......

### SynchronousQueue

待补充......

### LinkedTransferQueue

待补充......

### LinkedBlockingDeque

待补充......


### CompletableFuture使用

### 如何实现接口的幂等性

1）乐观锁
这种方法适合在更新的场景中，update t_goods set count = count -1 , version = version + 1 where good_id=2 and version = 1
根据version版本，也就是在操作库存前先获取当前商品的version版本号，然后操作的时候带上此version号。我们梳理下，我们第一次操作库存时，得到version为1，调用库存服务version变成了2；但返回给订单服务出现了问题，订单服务又一次发起调用库存服务，当订单服务传如的version还是1，再执行上面的sql语句时，就不会执行；因为version已经变为2了，where条件就不成立。这样就保证了不管调用几次，只会真正的处理一次。
乐观锁主要使用于处理读多写少的问题这种方法适合在更新的场景中，update t_goods set count = count -1 , version = version + 1 where good_id=2 and version = 1
根据version版本，也就是在操作库存前先获取当前商品的version版本号，然后操作的时候带上此version号。我们梳理下，我们第一次操作库存时，得到version为1，调用库存服务version变成了2；但返回给订单服务出现了问题，订单服务又一次发起调用库存服务，当订单服务传如的version还是1，再执行上面的sql语句时，就不会执行；因为version已经变为2了，where条件就不成立。这样就保证了不管调用几次，只会真正的处理一次。
乐观锁主要使用于处理读多写少的问题

2）唯一主键
这个机制是利用了数据库的主键唯一约束的特性，解决了在insert场景时幂等问题。但主键的要求不是自增的主键，这样就需要业务生成全局唯一的主键。

3）分布式锁
……
