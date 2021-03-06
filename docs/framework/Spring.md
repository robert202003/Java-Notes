- [SpringIoC和AOP](#SpringIoC和AOP)
- [BeanFactory和ApplicationContext的区别](#BeanFactory和ApplicationContext的区别)
- [Spring的bean作用范围](#Spring的bean作用范围)
- [Spring AOP 和 AspectJ AOP 有什么区别](#SpringAOP和AspectJAOP有什么区别)
- [SpringAOP原理](#SpringAOP原理)
- [SpringAOP五大通知类型](#SpringAOP五大通知类型)
- [SpringAOP五种通知的执行顺序](#SpringAOP五种通知的执行顺序)
- [Spring的生命周期](#Spring的生命周期)
- [Spring MVC原理](#SpringMVC原理)
- [Spring的事务传播机制](#Spring的事务传播机制)
- [Spring事务中的隔离级别有哪几种](#Spring事务中的隔离级别有哪几种)
- [Spring的依赖注入方式有哪些](#Spring的依赖注入方式有哪些)
- [@Transactional(rollbackFor = Exception.class)](#@Transactional的rollbackFor属性)
- [Spring容器的加载方式有哪些](#Spring容器的加载方式有哪些)
- [Spring的循环依赖问题，什么时候报异常](#Spring的循环依赖问题，什么时候报异常)
- [Spring管理事务的方式有几种](#Spring管理事务的方式有几种)
- [过滤器（filter）和拦截器（Interceptor）的区别](#过滤器（filter）和拦截器（Interceptor）的区别)
- [@Component 和 @Bean 的区别是什么？](#@Component和@Bean的区别是什么)
- [Spring框架中的单例bean是线程安全的吗？如果不安全如何解决](#Spring框架中的单例bean是线程安全的吗？如果不安全如何解决)
- [Spring框架中都用到了哪些设计模式](#Spring框架中都用到了哪些设计模式)
- [@Transactional什么时候会失效](#@Transactional什么时候会失效)
- [@Autowired和@Resource之间的区别](#@Autowired和@Resource之间的区别)

## SpringIoC和AOP

**IoC**
IoC（Inverse of Control:控制反转）是一种设计思想，就是 将原本在程序中手动创建对象的控制权，交由Spring框架来管理。 IoC 在其他语言中也有应用，并非 Spirng 特有。 IoC 容器是 Spring 用来实现 IoC 的载体， IoC 容器实际上就是个Map（key，value）,Map 中存放的是各种对象。

将对象之间的相互依赖关系交给 IOC 容器来管理，并由 IOC 容器完成对象的注入。这样可以很大程度上简化应用的开发，把应用从复杂的依赖关系中解放出来。 IOC 容器就像是一个工厂一样，当我们需要创建一个对象的时候，只需要配置好配置文件/注解即可，完全不用考虑对象是如何被创建出来的。 
在实际项目中一个 Service 类可能有几百甚至上千个类作为它的底层，假如我们需要实例化这个 Service，你可能要每次都要搞清这个 Service 
所有底层类的构造函数，这可能会把人逼疯。如果利用 IOC 的话，你只需要配置好，然后在需要的地方引用就行了，这大大增加了项目的可维护性且降低了开发难度

**AOP**
AOP(Aspect-Oriented Programming:面向切面编程)能够将那些与业务无关，却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可拓展性和可维护性。

Spring AOP就是基于动态代理的，如果要代理的对象，实现了某个接口，那么Spring AOP会使用JDK Proxy，去创建代理对象，而对于没有实现接口的对象，就无法使用 JDK Proxy 去进行代理了，这时候Spring AOP会使用Cglib ，这时候Spring AOP会使用 Cglib 生成一个被代理对象的子类来作为代理

## BeanFactory和ApplicationContext的区别？

**getBean()方法**

BeanFactory和ApplicationContext都可以通过调用getBean(“beanName”)获取bean的实例。同样是从Spring IOC容器中获取bean，但是它们提供的工作和功能有所不同。 

BeanFactory和ApplicationContext之间的区别是，前者仅在调用getBean()方法时实例化bean，而ApplicationContext在容器启动时实例化Singleton bean，而不等待getBean被调用。

| 特性  | BeanFactory | ApplicationContext | 
| :-------- | :------ | :---- | 
| Bean instantiation/wiring   | Yes   | Yes    | 
| BeanPostProcessor  |  手动   | 自动   | 
| BeanFactoryPostProcessor 注册  | 手动   | 自动   | 
| 国际化实现多语言配置（i18n）    | No   | Yes    | 
| ApplicationEvent publication  | No   | Yes    | 
| 实现  | DefaultListableBeanFactory和XmlBeanDefinitionReader   | ClassPath/FileSystem/WebXmlApplicationContext   | 
| 注解支持  | No   | Yes    | 


## Spring的bean作用范围？

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

## Spring AOP 和 AspectJ AOP 有什么区别？

Spring AOP 属于运行时增强，而 AspectJ 是编译时增强。 Spring AOP 基于动态代理(Proxying)，而 AspectJ 基于字节码操作(Bytecode Manipulation)。

Spring AOP 已经集成了 AspectJ ，AspectJ 应该算的上是 Java 生态系统中最完整的 AOP 框架了。AspectJ 相比于 Spring AOP 功能更加强大，但是 Spring AOP 相对来说更简单，

如果我们的切面比较少，那么两者性能差异不大。但是，当切面太多的话，最好选择 AspectJ ，它比Spring AOP 快很多。

## SpringAOP原理

SpringAOP的实现是基于代理模式 ,代理类型包括:静态代理、动态代理。

## SpringAOP五大通知类型

**1.前置通知**

在目标方法执行之前执行执行的通知。

前置通知方法，可以没有参数，也可以额外接收一个JoinPoint，Spring会自动将该对象传入，代表当前的连接点，通过该对象可以获取目标对象和目标方法相关的信息。

**2.环绕通知**

在目标方法执行之前和之后都可以执行额外代码的通知。

在环绕通知中必须显式的调用目标方法，目标方法才会执行，这个显式调用时通过ProceedingJoinPoint来实现的，可以在环绕通知中接收一个此类型的形参，spring容器会自动将该对象传入，注意这个参数必须处在环绕通知的第一个形参位置。

**要注意，只有环绕通知可以接收ProceedingJoinPoint，而其他通知只能接收JoinPoint。

环绕通知需要返回返回值，否则真正调用者将拿不到返回值，只能得到一个null。

**3.后置通知**

在目标方法执行之后执行的通知。

在后置通知中也可以选择性的接收一个JoinPoint来获取连接点的额外信息，但是这个参数必须处在参数列表的第一个。在后置通知中，还可以通过配置获取返回值，一定要保证JoinPoint处在参数列表的第一位，否则抛异常

**4.异常通知**

在目标方法抛出异常时执行的通知

可以配置传入JoinPoint获取目标对象和目标方法相关信息，但必须处在参数列表第一位。另外，还可以配置参数，让异常通知可以接收到目标方法抛出的异常对象。

**5.最终通知**

是在目标方法执行之后执行的通知。

和后置通知不同之处在于，后置通知是在方法正常返回后执行的通知，如果方法没有正常返-例如抛出异常，则后置通知不会执行。

而最终通知无论如何都会在目标方法调用过后执行，即使目标方法没有正常的执行完成。

另外，后置通知可以通过配置得到返回值，而最终通知无法得到。

最终通知也可以额外接收一个JoinPoint参数，来获取目标对象和目标方法相关信息，但一定要保证必须是第一个参数。

## SpringAOP五种通知的执行顺序

**1.在目标方法没有抛出异常的情况下**

前置通知

环绕通知的调用目标方法之前的代码

目标方法

环绕通知的调用目标方法之后的代码

后置通知

最终通知

**2.在目标方法抛出异常的情况下**

前置通知

环绕通知的调用目标方法之前的代码

目标方法 抛出异常 异常通知

最终通知

## Spring bean的生命周期

- Bean 容器找到配置文件中 Spring Bean 的定义，Bean 容器利用 Java Reflection API 创建一个Bean的实例。

- 如果涉及到一些属性值 利用 set()方法设置一些属性值。

- 如果 Bean 实现了 BeanNameAware 接口，调用 setBeanName()方法，传入Bean的名字。

- 如果 Bean 实现了 BeanClassLoaderAware 接口，调用 setBeanClassLoader()方法，传入 ClassLoader对象的实例。

- 如果Bean实现了 BeanFactoryAware 接口，调用 setBeanClassLoader()方法，传入 ClassLoader对象的实例。

- 与上面的类似，如果实现了其他 *.Aware接口，就调用相应的方法。

- 如果有和加载这个 Bean 的 Spring 容器相关的 BeanPostProcessor 对象，执行postProcessBeforeInitialization() 方法

- 如果Bean实现了InitializingBean接口，执行afterPropertiesSet()方法。

- 如果 Bean 在配置文件中的定义包含 init-method 属性，执行指定的方法。

- 如果有和加载这个 Bean的 Spring 容器相关的 BeanPostProcessor 对象，执行postProcessAfterInitialization() 方法

- 当要销毁 Bean 的时候，如果 Bean 实现了 DisposableBean 接口，执行 destroy() 方法。

- 当要销毁 Bean 的时候，如果 Bean 在配置文件中的定义包含 destroy-method 属性，执行指定的方法。

## Spring MVC原理

1.用户发起请求到前端控制器dispatcherServlet，  

2.前端控制器请求处理器映射器handlerMapping来查找相应handler（可以根据xml配置或注解配置查找），

3.处理器映射器handlerMapping返回给前端控制器handler，

4.前端控制器调用处理器适配器handlerAdapter去执行handler

5.处理器适配器handlerAdapter执行handler

6.handler执行完成给处理器适配器返回ModelAndView

7.处理器适配器handlerAdapter向前端控制器返回ModelAndView(springMVC的一个底层对象，包括model和view)

8.前端控制器请求视图解析器进行视图解析

9.视图解析器向前端控制器返回view 

10.前端控制器进行视图渲染

11.前端控制器向用户响应结果。

## Spring的事务传播机制

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

也可以这样记忆：

Spring事务的传播机制共7中,可以分为3组+1个特殊来分析或者记忆  

**1).REQUIRE组**  

1.REQUIRED:当前存在事务则使用当前的事务, 当前不存在事务则创建一个新的事务

2.REQUIRES_NEW:创建新事务, 如果已经存在事务, 则把已存在的事务挂起  

**2).SUPPORT组**

1.SUPPORTS:支持事务. 如果当前存在事务则加入该事务, 如果不存在事务则以无事务状态执行

2.NOT_SUPPORTED:不支持事务. 在无事务状态下执行,如果已经存在事务则挂起已存在的事务

**3).Exception组** 

1.MANDATORY:必须在事务中执行, 如果当前不存在事务, 则抛出异常

2.NEVER: 不可在事务中执行, 如果当前存在事务, 则抛出异常 
 
**4).NESTED:嵌套事务**

.如果当前存在事务, 则嵌套执行, 如果当前不存在事务, 则开启新事务。

## Spring事务中的隔离级别有哪几种？

- TransactionDefinition 接口中定义了五个表示隔离级别的常量：

- TransactionDefinition.ISOLATION_DEFAULT: 使用后端数据库默认的隔离级别，Mysql 默认采用的 REPEATABLE_READ隔离级别 Oracle 默认采用的 READ_COMMITTED隔离级别.

- TransactionDefinition.ISOLATION_READ_UNCOMMITTED: 最低的隔离级别，允许读取尚未提交的数据变更，可能会导致脏读、幻读或不可重复读

- TransactionDefinition.ISOLATION_READ_COMMITTED: 允许读取并发事务已经提交的数据，可以阻止脏读，但是幻读或不可重复读仍有可能发生

- TransactionDefinition.ISOLATION_REPEATABLE_READ: 对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改，可以阻止脏读和不可重复读，但幻读仍有可能发生。

- TransactionDefinition.ISOLATION_SERIALIZABLE: 最高的隔离级别，完全服从ACID的隔离级别。所有的事务依次逐个执行，这样事务之间就完全不可能产生干扰，也就是说，该级别可以防止脏读、不可重复读以及幻读。但是这将严重影响程序的性能。通常情况下也不会用到该级别。

## Spring的依赖注入方式有哪些？

- @Autowired：自动装配  基于@Autowired的自动装配，默认是根据类型注入，可以用于构造器、字段、方法注入
- setter 方法注入
- 构造器注入
- 静态工厂的方法注入
- 实例工厂的方法注入

## Transactional的rollbackFor属性

- 当@Transactional注解作用于类上时，该类的所有 public 方法将都具有该类型的事务属性，同时，我们也可以在方法级别使用该标注来覆盖类级别的定义。如果类或者方法加了这个注解，那么这个类里面的方法抛出异常，就会回滚，数据库里面的数据也会回滚。

- 在@Transactional注解中如果不配置rollbackFor属性,那么事务只会在遇到RuntimeException的时候才会回滚,加上rollbackFor=Exception.class,可以让事务在遇到非运行时异常时也回滚。

## Spring容器的加载方式有哪些？

- 类路径获得配置文件（相对路径）

```java
public static void main(String[] args) {
        //通过ClassPathXmlApplicationContext加载beans.xml文件，填入的相对路径，默认路径是src
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        //通过context获取user对象
        User user = (User) context.getBean("user");
        //打印user
        System.out.println(user);
    }
```
- 文件系统路径获得配置文件

```java
public static void main(String[] args) {
        //通过FileSystemXmlApplicationContext加载beans.xml文件，填入的绝对路径
        //如果你是windows，直接填入绝对路径，如果是mac，得在绝对路径前加一个斜杠
        ApplicationContext context = new FileSystemXmlApplicationContext("//Users/hestyle/IdeaProjects/Spring Project 01/src/beans.xml");
        //通过context获取user对象
        User user = (User) context.getBean("user");
        //打印user
        System.out.println(user);
    }
```
- 使用BeanFactory（不推荐）

```java
//通过XmlBeanFactory加载beans.xml文件，填入的绝对路径
        String path = "/Users/hestyle/IdeaProjects/Spring Project 01/src/beans.xml";
        BeanFactory factory = new XmlBeanFactory(new FileSystemResource(path));
        //通过context获取user对象
        User user = (User) factory.getBean("user");
        //打印user
        System.out.println(user);
```

## Spring的循环依赖问题，什么时候报异常

## Spring管理事务的方式有几种？

编程式事务，在代码中硬编码。(不推荐使用)

声明式事务，在配置文件中配置（推荐使用）

声明式事务又分为两种：

基于XML的声明式事务和基于注解的声明式事务

## 过滤器（filter）和拦截器（Interceptor）的区别

1.拦截器是基于java的反射机制的，而过滤器是基于函数回调。  
2.拦截器不依赖与servlet容器，过滤器依赖与servlet容器。  
3.拦截器只能对action请求起作用，而过滤器则可以对几乎所有的请求起作用。  
4.拦截器可以访问action上下文、值栈里的对象，而过滤器不能访问。  
5.在action的生命周期中，拦截器可以多次被调用，而过滤器只能在容器初始化时被调用一次。  
6.拦截器可以获取IOC容器中的各个bean，而过滤器就不行，这点很重要，在拦截器里注入一个service，可以调用业务逻辑。  
总结：过滤器包裹住servlet，servlet包裹住拦截器。

## @Component 和 @Bean 的区别是什么？

作用对象不同: @Component 注解作用于类，而@Bean注解作用于方法。

@Component通常是通过类路径扫描来自动侦测以及自动装配到Spring容器中（我们可以使用 @ComponentScan 注解定义要扫描的路径从中找出标识了需要装配的类自动装配到 Spring 的 bean 容器中）。@Bean 注解通常是我们在标有该注解的方法中定义产生这个 bean,@Bean告诉了Spring这是某个类的示例，当我需要用它的时候还给我。

@Bean 注解比 Component 注解的自定义性更强，而且很多地方我们只能通过 @Bean 注解来注册bean。比如当我们引用第三方库中的类需要装配到 Spring容器时，则只能通过 @Bean来实现。

## Spring框架中的单例bean是线程安全的吗？如果不安全如何解决

不是，Spring框架中的单例bean不是线程安全的。
常见的有两种解决办法：

在Bean对象中尽量避免定义可变的成员变量（不太现实）。

在类中定义一个ThreadLocal成员变量，将需要的可变成员变量保存在 ThreadLocal 中（推荐的一种方式）。

## Spring框架中都用到了哪些设计模式？

（1）工厂模式：BeanFactory就是简单工厂模式的体现，用来创建对象的实例。  
（2）单例模式：Bean默认为单例模式。  
（3）代理模式：Spring的AOP功能用到了JDK的动态代理和CGLIB字节码生成技术。  
（4）模板方法：用来解决代码重复的问题。比如. RestTemplate, JmsTemplate, JpaTemplate。  
（5）观察者模式：定义对象键一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都会得到通知被制动更新，如Spring中listener的实现--ApplicationListener。

## @Transactional什么时候会失效

1）service类标签(一般不建议在接口上)上添加@Transactional，可以将整个类纳入spring事务管理，在每个业务方法执行时都会开启一个事务，不过这些事务采用相同的管理方式。

2）@Transactional注解只能应用到public可见度的方法上。如果应用在protected、private或者package可见度的方法上，也不会报错，不过事务设置不会起作用。

3）默认情况下，Spring会对unchecked异常进行事务回滚；如果是checked异常则不回滚。

4）只读事务： @Transactional(propagation=Propagation.NOT_SUPPORTED,readOnly=true) 只读标志只在事务启动时应用，否则即使配置也会被忽略。

## @Autowired和@Resource之间的区别

(1) @Autowired默认是按照类型装配注入的，默认情况下它要求依赖对象必须存在（可以设置它required属性为false）。  
(2) @Resource默认是按照名称来装配注入的，只有当找不到与名称匹配的bean才会按照类型来装配注入。