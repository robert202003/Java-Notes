- [JVM内存模型](#JVM内存模型)
- [类文件结构](#类文件结构)
- [虚拟机类加载机制](#虚拟机类加载机制)
  - [类加载的时机](#类加载的时机)
  - [类加载过程](#类加载过程)
  - [类加载器](#类加载器有哪些)
    - [类与类加载器](#类与类加载器)  
    - [双亲委派机制](#双亲委派机制)  
    - [破坏双亲委派机制](#破坏双亲委派机制)  

## JVM内存模型


## 虚拟机类加载机制


## 什么是双亲委派机制？

- 双亲委派模型的工作流程是：如果一个类加载器收到了类加载的请求，它首先不会自己去加载这个类，而是把请求委派给父加载器去完成，
依次向上，因此，所有的类加载请求最终都应该被传递到顶层的启动类加载器中，只有当父加载器没有找到所需的类时，子加载器才会尝试去加载该类。

**双亲委派机制:**

1、 当 AppClassLoader 加载一个 class 时，它首先不会自己去尝试加载这个类，而是把类加载请求委派给父类加载器 ExtClassLoader 去完成。

2、 当 ExtClassLoader 加载一个 class 时，它首先也不会自己去尝试加载这个类，而是把类加载请求委派给 BootStrapClassLoader 去完成。

3、 如果 BootStrapClassLoader 加载失败，会使用 ExtClassLoader 来尝试加载；

4、 若 ExtClassLoader 也加载失败，则会使用 AppClassLoader 来加载，如果 AppClassLoader 也加载失败，则会报出异常 ClassNotFoundException。


## 类加载的时机

遇到new, getstatic, putstatic, invokestatic这四条字节码指令的时候，如果类型还没有被初始化，则需要初始化。

new :实例化对象（对象实例调用表达式所创建的对象）

getstatic/putstatic: 读取/设置类的静态字段（被final修饰的静态常量除外）

invokestatic: 调用类的静态方法

## 类加载过程


## 类加载器有哪些？

JVM 中内置了三个重要的 ClassLoader，除了 BootstrapClassLoader 其他类加载器均由 Java 实现且全部继承自java.lang.ClassLoader：

- BootstrapClassLoader(启动类加载器) ：最顶层的加载类，由C++实现，负责加载 %JAVA_HOME%/lib目录下的jar包和类或者或被 -Xbootclasspath参数指定的路径中的所有类。
- ExtensionClassLoader(扩展类加载器) ：主要负责加载目录 %JRE_HOME%/lib/ext 目录下的jar包和类，或被 java.ext.dirs 系统变量所指定的路径下的jar包。
- AppClassLoader(应用程序类加载器) ：面向我们用户的加载器，负责加载当前应用classpath下的所有jar包和类。



## 如何查看 JVM 当前使用的是什么垃圾收集器？

-XX:+PrintCommandLineFlags 参数可以打印出所选垃圾收集器和堆空间大小等设置

如果开启了 GC 日志详细信息，里面也会包含各代使用的垃圾收集器的简称