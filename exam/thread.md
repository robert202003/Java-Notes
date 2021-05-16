## 1.进程和线程的区别（难度：★★）
- 进程是代码在数据集合上的一次运行活动，是系统进行资源分配和调度的基本单元。
- 线程则是进程的执行路径，一个进程至少有一个线程，线程是CPU分配的基本单元。

## 2.线程的创建方式（难度：★）
- 继承Thread类
- 实现runnable接口
- 实现callable接口，有返回值

## 3.Java中sleep和wait的区别（难度：★）

- sleep方法是Thread类的静态方法，wait方法是Object类的成员方法。  
- sleep方法不会释放lock，但是wait会释放lock，而且会加入等待队列中  
- sleep方法不依赖于同步器synchronized，但是wait需要依赖synchronized关键字  
- sleep方法有可能会抛出异常，所以需要进行异常处理；wait方法不需要处理  
- sleep方法可以在任何地方使用；wait方法只能在同步方法和同步代码块中使用  

## 4.synchronized实现原理（难度：★★★★）

**1. 作用**  
原子性：确保线程互斥的访问同步代码；
可见性：保证共享变量的修改能够及时可见，其实是通过Java内存模型中的 “对一个变量unlock操作之前，必须要同步到主内存中；如果对一个变量进行lock操作，则将会清空工作内存中此变量的值，在执行引擎使用此变量前，需要重新从主内存中load操作或assign操作初始化变量值” 来保证的；
有序性：有效解决重排序问题，即 “一个unlock操作先行发生(happen-before)于后面对同一个锁的lock操作”；

**2. 三种用法** 
当synchronized作用在实例方法时，监视器锁（monitor）便是对象实例（this）；
当synchronized作用在静态方法时，监视器锁（monitor）便是对象的Class实例，因为Class数据存在于永久代，因此静态方法锁相当于该类的一个全局锁；
当synchronized作用在某一个对象实例时，监视器锁（monitor）便是括号括起来的对象实例；

**3. 实现原理** 
monitorenter：每个对象都是一个监视器锁（monitor）。当monitor被占用时就会处于锁定状态，线程执行monitorenter指令时尝试获取monitor的所有权，过程如下：

如果monitor的进入数为0，则该线程进入monitor，然后将进入数设置为1，该线程即为monitor的所有者；
如果线程已经占有该monitor，只是重新进入，则进入monitor的进入数加1；
如果其他线程已经占用了monitor，则该线程进入阻塞状态，直到monitor的进入数为0，再重新尝试获取monitor的所有权；
monitorexit：执行monitorexit的线程必须是objectref所对应的monitor的所有者。指令执行时，monitor的进入数减1，如果减1后进入数为0，那线程退出monitor，不再是这个monitor的所有者。其他被这个monitor阻塞的线程可以尝试去获取这个 monitor 的所有权。

**4. 锁升级过程** 
待补充......

## 5.volatile理解难度：（难度：★★★）
1）保证内存可见性

通俗来说就是，线程A对一个volatile变量的修改，对于其它线程来说是可见的，即线程每次获取volatile变量的值都是最新的。

当一个变量被 volatile 修饰时，任何线程对它的写操作都会立即刷新到主内存中，并且会强制让缓存了该变量的线程中的数据清空，必须从主内存重新读取最新数据。

2）禁止指令重排序
禁止指令重排序的原因：
JMM是允许编译器和处理器对指令重排序的，但是规定了as-if-serial语义，程序的执行结果不能改变

说明：volatile并不能保证原子性

## 6.synchronized和volatile的区别？（难度：★★★）
一旦一个共享变量（类的成员变量、类的静态成员变量）被volatile修饰之后，那么就具备了两层语义：   
1.保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的。  
2.禁止进行指令重排序。    
   volatile本质是在告诉jvm当前变量在寄存器（工作内存）中的值是不确定的，需要从主存中读取；  
   synchronized则是锁定当前变量，只有当前线程可以访问该变量，其他线程被阻塞住。  

- volatile仅能使用在变量级别；synchronized则可以使用在变量、方法、和类级别的。  
- volatile仅能实现变量的修改可见性，并不能保证原子性；synchronized则可以保证变量的修改可见性和原子性。  
- volatile不会造成线程的阻塞；synchronized可能会造成线程的阻塞。  
- volatile标记的变量不会被编译器优化；synchronized标记的变量可以被编译器优化。  

## 7.java对象头包含哪些内容（难度：★★★）
待补充......

## 8.ReentrantLock实现原理（难度：★★★★）
待补充......

## 9.ReentrantReadWriteLock原理（难度：★★★★）

## 10.synchronized和ReentrantLock的区别（难度：★★★）
synchronized是和if、else、for、while一样的关键字，ReentrantLock是类，这是二者的本质区别。既然ReentrantLock是类，那么它就提供了比synchronized更多更灵活的特性，可以被继承、可以有方法、可以有各种各样的类变量，ReentrantLock比synchronized的扩展性体现在几点上： 
- ReentrantLock可以对获取锁的等待时间进行设置，这样就避免了死锁 
- ReentrantLock可以获取各种锁的信息 
- ReentrantLock可以灵活地实现多路通知 
- 另外，二者的锁机制其实也是不一样的:ReentrantLock底层调用的是Unsafe的park方法加锁，synchronized操作的应该是对象头中mark word.

## 11.ThreadLocal实现原理和内存泄露（难度：★★★）
待补充......

## 12.ThreadLocalRandom原理（难度：★★★）
待补充......

## 13.乐观锁和悲观锁区别（难度：★★）
- 乐观锁：乐观锁认为竞争不总是会发生，因此它不需要持有锁，将比较-替换这两个动作作为一个原子操作尝试去修改内存中的变量，如果失败则表示发生冲突，那么就应该有相应的重试逻辑。
- 悲观锁：悲观锁认为竞争总是会发生，因此每次对某资源进行操作时，都会持有一个独占的锁，就像synchronized，不管三七二十一，直接上了锁就操作资源了。

## 14.什么是可重入锁？（难度：★★★）
待补充......

## 15.什么是自旋锁？（难度：★★★）
待补充......

## 16.公平锁和非公平锁的区别（难度：★★★）
待补充......

## 17.独占锁和共享锁的区别（难度：★★★）
待补充......

## 18.如何实现接口的幂等性（难度：★★★）
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
## 19.AQS底层原理（难度：★★★★★）

## 20.理解CAS（难度：★★★）

## 21.CountDownLatch原理和使用（难度：★★★★）

**1. 原理**  
CountDownLatch是使用AQS实现的，使用AQS的状态变量来存放计数器的值。首先在初始化CountDownLatch时设置状态值（计算器值），当多个线程调用countdown
方法时实际是原子性递减AQS的状态值。当线程调用await方法后当前线程会被放入AQS的阻塞队列等待计数器为0再返回。其他线程调用countdown方法让计数器值递减1，
当计数器值变为0时，当前线程还要调用AQS的doReleaseShared方法来激活由于调用await()方法而被阻塞的线程。

**2. 使用**

## 22.CyclicBarrier原理和使用（难度：★★★★）

## 23.Semaphore原理和使用（难度：★★★★）

## 24.ThreadPoolExecutor线程池核心参数和原理（难度：★★★）

## 25.CompletableFuture使用

## 26.ConcurrentLinkedQueue原理（难度：★★★★）

## 27.LinkedBlockingQueue原理（难度：★★★★）

## 28.ArrayBlockingQueue原理（难度：★★★★）

## 29.PriorityBlockingQueue原理（难度：★★★★）

## 30.DelayQueue原理（难度：★★★★）

## 31.SynchronousQueue（难度：★★★★）

## 32.LinkedTransferQueue（难度：★★★★）

## 33.LinkedBlockingDeque（难度：★★★★）