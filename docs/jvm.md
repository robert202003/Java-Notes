# JVM面试题

## 操作系统的内存管理机制

## 虚拟机的类的加载机制（难度：★★★）

## JVM内存模型（难度：★★★）

## 什么是双亲委派机制？（难度：★★★）

双亲委派模型的工作流程是：如果一个类加载器收到了类加载的请求，它首先不会自己去加载这个类，
而是把请求委派给父加载器去完成，依次向上，因此，所有的类加载请求最终都应该被传递到顶层的启动类加载器中，
只有当父加载器没有找到所需的类时，子加载器才会尝试去加载该类。

双亲委派机制:

1、 当 AppClassLoader 加载一个 class 时，它首先不会自己去尝试加载这个类，而是把类加载请求委派给父类加载器 ExtClassLoader 去完成。

2、 当 ExtClassLoader 加载一个 class 时，它首先也不会自己去尝试加载这个类，而是把类加载请求委派给 BootStrapClassLoader 去完成。

3、 如果 BootStrapClassLoader 加载失败，会使用 ExtClassLoader 来尝试加载；

4、 若 ExtClassLoader 也加载失败，则会使用 AppClassLoader 来加载，如果 AppClassLoader 也加载失败，则会报出异常 ClassNotFoundException。
## 类加载器有哪些？（难度：★★★）

## 如何查看 JVM 当前使用的是什么垃圾收集器？
-XX:+PrintCommandLineFlags 参数可以打印出所选垃圾收集器和堆空间大小等设置

如果开启了 GC 日志详细信息，里面也会包含各代使用的垃圾收集器的简称