# 编程实战

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