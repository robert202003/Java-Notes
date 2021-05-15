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


# 二、JVM面试题

## 1.虚拟机的类的加载机制（难度星数：★★★）

## 2.JVM运行时数据区域（难度星数：★★★）

## 3.JVM内存模型（难度星数：★★★）

## 4.什么是双亲委派机制？（难度星数：★★★）

##  5.类加载器有哪些？（难度星数：★★★）

## 6.java类的加载时机（难度星数：★★★）

# 三、Spring面试题

## 1.谈谈自己对于 Spring IoC 和 AOP 的理解
**IoC**
IoC（Inverse of Control:控制反转）是一种设计思想，就是 将原本在程序中手动创建对象的控制权，交由Spring框架来管理。 IoC 在其他语言中也有应用，并非 Spirng 特有。 IoC 容器是 Spring 用来实现 IoC 的载体， IoC 容器实际上就是个Map（key，value）,Map 中存放的是各种对象。

将对象之间的相互依赖关系交给 IOC 容器来管理，并由 IOC 容器完成对象的注入。这样可以很大程度上简化应用的开发，把应用从复杂的依赖关系中解放出来。 IOC 容器就像是一个工厂一样，当我们需要创建一个对象的时候，只需要配置好配置文件/注解即可，完全不用考虑对象是如何被创建出来的。 
在实际项目中一个 Service 类可能有几百甚至上千个类作为它的底层，假如我们需要实例化这个 Service，你可能要每次都要搞清这个 Service 
所有底层类的构造函数，这可能会把人逼疯。如果利用 IOC 的话，你只需要配置好，然后在需要的地方引用就行了，这大大增加了项目的可维护性且降低了开发难度

**AOP**
AOP(Aspect-Oriented Programming:面向切面编程)能够将那些与业务无关，却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可拓展性和可维护性。

Spring AOP就是基于动态代理的，如果要代理的对象，实现了某个接口，那么Spring AOP会使用JDK Proxy，去创建代理对象，而对于没有实现接口的对象，就无法使用 JDK Proxy 去进行代理了，这时候Spring AOP会使用Cglib ，这时候Spring AOP会使用 Cglib 生成一个被代理对象的子类来作为代理

## 2.BeanFactory和ApplicationContext的区别（难度星数：★★★）

## 3.Spring的bean作用范围有哪些（难度星数：★★）

**singleton （默认值）**  
将每个Spring IoC容器的单个bean定义范围限定为单个对象实例。

**prototype** 

将单个bean定义的作用域限定为任意数量的对象实例。

**request** 

将单个bean定义的范围限定为单个HTTP请求的生命周期。也就是说，每个HTTP请求都有一个在单个bean定义的后面创建的bean实例。仅在可感知网络的Spring上下文中有效ApplicationContext。

**session** 

将单个bean定义的范围限定为HTTP的生命周期Session。仅在可感知网络的Spring上下文中有效ApplicationContext。

**application**  

将单个bean定义的作用域限定为的生命周期ServletContext。仅在可感知网络的Spring上下文中有效ApplicationContext。

**websocket**

将单个bean定义的作用域限定为的生命周期WebSocket。仅在可感知网络的Spring上下文中有效ApplicationContext。

## 4.Spring AOP 和 AspectJ AOP 有什么区别？
Spring AOP 属于运行时增强，而 AspectJ 是编译时增强。 Spring AOP 基于代理(Proxying)，而 AspectJ 基于字节码操作(Bytecode Manipulation)。

Spring AOP 已经集成了 AspectJ ，AspectJ 应该算的上是 Java 生态系统中最完整的 AOP 框架了。AspectJ 相比于 Spring AOP 功能更加强大，但是 Spring AOP 相对来说更简单，

如果我们的切面比较少，那么两者性能差异不大。但是，当切面太多的话，最好选择 AspectJ ，它比Spring AOP 快很多。


## 5.Spring的生命周期（难度星数：★★★★）

- Bean 容器找到配置文件中 Spring Bean 的定义。

- Bean 容器利用 Java Reflection API 创建一个Bean的实例。

- 如果涉及到一些属性值 利用 set()方法设置一些属性值。

- 如果 Bean 实现了 BeanNameAware 接口，调用 setBeanName()方法，传入Bean的名字。

- 如果 Bean 实现了 BeanClassLoaderAware 接口，调用 setBeanClassLoader()方法，传入 ClassLoader对象的实例。

- 如果Bean实现了 BeanFactoryAware 接口，调用 setBeanClassLoader()方法，传入 ClassLoade r对象的实例。

- 与上面的类似，如果实现了其他 *.Aware接口，就调用相应的方法。

- 如果有和加载这个 Bean 的 Spring 容器相关的 BeanPostProcessor 对象，执行postProcessBeforeInitialization() 方法

- 如果Bean实现了InitializingBean接口，执行afterPropertiesSet()方法。

- 如果 Bean 在配置文件中的定义包含 init-method 属性，执行指定的方法。

- 如果有和加载这个 Bean的 Spring 容器相关的 BeanPostProcessor 对象，执行postProcessAfterInitialization() 方法

- 当要销毁 Bean 的时候，如果 Bean 实现了 DisposableBean 接口，执行 destroy() 方法。

- 当要销毁 Bean 的时候，如果 Bean 在配置文件中的定义包含 destroy-method 属性，执行指定的方法。

## 6.Spring MVC原理
- 客户端（浏览器）发送请求，直接请求到 DispatcherServlet。

- DispatcherServlet 根据请求信息调用 HandlerMapping，解析请求对应的 Handler。

- 解析到对应的 Handler（也就是我们平常说的 Controller 控制器）后，开始由 HandlerAdapter 适配器处理。

- HandlerAdapter 会根据 Handler来调用真正的处理器开处理请求，并处理相应的业务逻辑。

- 处理器处理完业务后，会返回一个 ModelAndView 对象，Model 是返回的数据对象，View 是个逻辑上的 View。

- ViewResolver 会根据逻辑 View 查找实际的 View。

- DispaterServlet 把返回的 Model 传给 View（视图渲染）。

- 把 View 返回给请求者（浏览器）

## 7.Spring的事务传播机制（难度星数：★★★）
支持当前事务的情况：

- TransactionDefinition.PROPAGATION_REQUIRED： 如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务。

- TransactionDefinition.PROPAGATION_SUPPORTS： 如果当前存在事务，则加入该事务；如果当前没有事务，则以非事务的方式继续运行。

- TransactionDefinition.PROPAGATION_MANDATORY： 如果当前存在事务，则加入该事务；如果当前没有事务，则抛出异常。（mandatory：强制性）

不支持当前事务的情况：

- TransactionDefinition.PROPAGATION_REQUIRES_NEW： 创建一个新的事务，如果当前存在事务，则把当前事务挂起。

- TransactionDefinition.PROPAGATION_NOT_SUPPORTED： 以非事务方式运行，如果当前存在事务，则把当前事务挂起。

- TransactionDefinition.PROPAGATION_NEVER： 以非事务方式运行，如果当前存在事务，则抛出异常。

其他情况：

- TransactionDefinition.PROPAGATION_NESTED： 如果当前存在事务，则创建一个事务作为当前事务的嵌套事务来运行；如果当前没有事务，则该取值等价于TransactionDefinition.PROPAGATION_REQUIRED。

## 8.Spring 事务中的隔离级别有哪几种?
- TransactionDefinition 接口中定义了五个表示隔离级别的常量：

- TransactionDefinition.ISOLATION_DEFAULT: 使用后端数据库默认的隔离级别，Mysql 默认采用的 REPEATABLE_READ隔离级别 Oracle 默认采用的 READ_COMMITTED隔离级别.

- TransactionDefinition.ISOLATION_READ_UNCOMMITTED: 最低的隔离级别，允许读取尚未提交的数据变更，可能会导致脏读、幻读或不可重复读

- TransactionDefinition.ISOLATION_READ_COMMITTED: 允许读取并发事务已经提交的数据，可以阻止脏读，但是幻读或不可重复读仍有可能发生

- TransactionDefinition.ISOLATION_REPEATABLE_READ: 对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改，可以阻止脏读和不可重复读，但幻读仍有可能发生。

- TransactionDefinition.ISOLATION_SERIALIZABLE: 最高的隔离级别，完全服从ACID的隔离级别。所有的事务依次逐个执行，这样事务之间就完全不可能产生干扰，也就是说，该级别可以防止脏读、不可重复读以及幻读。但是这将严重影响程序的性能。通常情况下也不会用到该级别。

## 9.Spring的注入方式有哪些？（难度星数：★★）

## 10.Spring的加载方式有哪些？（难度星数：★★）

## 11.Spring的循环依赖问题，什么时候报异常（难度星数：★★★★）

## 12.Spring 管理事务的方式有几种？
编程式事务，在代码中硬编码。(不推荐使用)

声明式事务，在配置文件中配置（推荐使用）

声明式事务又分为两种：

基于XML的声明式事务和基于注解的声明式事务

## 13.过滤器（filter）和拦截器（Interceptor）的区别

1.拦截器是基于java的反射机制的，而过滤器是基于函数回调。  
2.拦截器不依赖与servlet容器，过滤器依赖与servlet容器。  
3.拦截器只能对action请求起作用，而过滤器则可以对几乎所有的请求起作用。  
4.拦截器可以访问action上下文、值栈里的对象，而过滤器不能访问。  
5.在action的生命周期中，拦截器可以多次被调用，而过滤器只能在容器初始化时被调用一次。  
6.拦截器可以获取IOC容器中的各个bean，而过滤器就不行，这点很重要，在拦截器里注入一个service，可以调用业务逻辑。  
总结：过滤器包裹住servlet，servlet包裹住拦截器。

## 14.@Component 和 @Bean 的区别是什么？
作用对象不同: @Component 注解作用于类，而@Bean注解作用于方法。

@Component通常是通过类路径扫描来自动侦测以及自动装配到Spring容器中（我们可以使用 @ComponentScan 注解定义要扫描的路径从中找出标识了需要装配的类自动装配到 Spring 的 bean 容器中）。@Bean 注解通常是我们在标有该注解的方法中定义产生这个 bean,@Bean告诉了Spring这是某个类的示例，当我需要用它的时候还给我。

@Bean 注解比 Component 注解的自定义性更强，而且很多地方我们只能通过 @Bean 注解来注册bean。比如当我们引用第三方库中的类需要装配到 Spring容器时，则只能通过 @Bean来实现。

## 15.Spring框架中的单例bean是线程安全的吗？如果不安全如何解决
不是，Spring框架中的单例bean不是线程安全的。
常见的有两种解决办法：

在Bean对象中尽量避免定义可变的成员变量（不太现实）。

在类中定义一个ThreadLocal成员变量，将需要的可变成员变量保存在 ThreadLocal 中（推荐的一种方式）。


# 四、数据库面试题

## 1.查询重复的名字（难度星数：★）

## 2.乐观锁和悲观锁的实现（难度星数：★★★）

## 3.spring事务的隔离级别（难度星数：★★★）

## 4.索引的底层数据结构有哪些？有什么区别（难度星数：★★★★）

## 5.MySQL性能如何优化（难度星数：★★★）

## 6.B +树比B树做了哪些优化（难度星数：★★★★★）

# 五、Redis面试题

## 1.Redis的数据类型有哪些（难度星数：★★）

Redis 有 5 种基础数据结构，它们分别是：string(字符串)、list(列表)、hash(字典)、set(集合) 和 zset(有序集合)。

## 2.Redis的常见命令有哪些（难度星数：★★★）

## 3.如何设置Redis的key的过期时间（难度星数：★★）

## 4.Redis持久化的方式有哪些？有什么区别（难度星数：★★★）

## 5.redis的key过期策略（难度星数：★★★）

## 6.Redis集群的方法有哪些（难度星数：★★★）

## 7.Redis的缓存穿透（难度星数：★★★）

## 8.Redis的雪崩效应，如何解决（难度星数：★★★）

# 六、编程实战

## 1.递归读取文件夹下的文件，代码怎么实现？

```java
import java.io.File;
import java.util.ArrayList;
import java.util.List;

public class ListDirectoryFileName {
   public static void showFileName(String fileDir) {
           if (file == null) return;
           File[] files = new File(fileDir).listFiles();
           for (File a : files) {
               System.out.println(a.getAbsolutePath());
               if (a.isDirectory()) {
                   showFileName(a);
               }
           }
       }
}
```
## 2.手写单例

饿汉模式：
```java
public class Singleton {  
    private Singleton() {}  
    private static final Singleton single = new Singleton();  
    //静态工厂方法   
    public static Singleton getInstance() {  
        return single;  
    }  
}  
```
懒汉模式：
```java
public class Singleton {  
    private Singleton() {}  
    private static Singleton single = null;  
    //静态工厂方法   
    public static synchronized Singleton getInstance() {  
         if (single == null) {    
             single = new Singleton();  
         }    
        return single;  
    }  
}  
```

2）双重检查锁定
```java
public static Singleton getInstance() {  
        if (singleton == null) {    
            synchronized (Singleton.class) {    
               if (singleton == null) {    
                  singleton = new Singleton();   
               }    
            }    
        }    
        return singleton;   
 }  
```

3）静态内部类
```java
public class Singleton {    
    private static class LazyHolder {    
       private static final Singleton INSTANCE = new Singleton();    
    }    
    private Singleton (){}    
    public static final Singleton getInstance() {    
       return LazyHolder.INSTANCE;    
    }    
}  
```
## 3.请写一个死锁程序

## 4.Java实现两个线程交替打印1-100



