## Java多线程编程

### 概念

* 进程：相当于进程管理器当中的一个程序，例如QQ
* 线程：相当于程序当中的诸多功能，例如QQ当中同时视频和语音聊天

### 使用多线程

* 继承Thread

```java
class Mythread extend Thread{}
```

* 实现Runnable

```java
class Mythread implements Runnable{}
```

* 实例变量与线程安全

```java
public class Share extends Thread{
    private int count=5;
    @Override
    public void run() {
        while (count>0)
        {
            count--;
            System.out.println("count="+count);
        }
    }
    public Share(String name)
    {
        super();
        this.setName(name);
    }
}
//sunchronized为了线程安全，在互斥区枷锁
public class shar1 extends Thread{
    private int count=5;
    @Override
    synchronized public void run()
    {
        count--;
    }
}
```

* currentThread()方法：__返回代码段正在被哪个线程调用__
* isAlive()：__判断当前线程是否还存活__
* sleep()：__让当前执行的线程休眠__

### 暂停线程

* suspend()：暂停线程
* resume()：恢复线程

### yield方法

* yield和sleep的区别

  > 1. sleep会导致当前线程暂停指定的时间，没cpu时间片的消耗
  > 2. yield会导致线程上下文切换
  > 3. sleep会使线程暂停block,在给定时间释放cpu资源
  > 4. yield使RUNNING进入RUNNABLE
  > 5. sleep百分之百完成线程的休眠，yield提示不一定
  > 6. 一个线程sleep另一个线程调用interrupt会捕获中断信号
  
* wait和sleep的区别

  > 1.两者都可以使线程处于阻塞状态
  >
  > 2.两者均是可中断方法，被中断后都会收到异常
  >
  > 3.wait是object的方法，而sleep是Thread独有
  >
  > 4.wait必须在同步方法中使用，而sleep不需要
  >
  > 5.wait方法会释放monitor的锁，而sleep不会
  >
  > 6.sleep会主动退出阻塞，而wait方法不会，需要其他线程中断


### 线程优先级

* setPriority()：设置线程级别
* 线程可以继承，且继承的级别一样
* 线程优先级具有不确定性和随机性



### Lambal

* 普通方法

  > 代码实例
  >
  > ```java
  > public class TestLambda {
  >     public static void main(String[] args) {
  >         Random r = new Random();
  >         List<Hero> heros = new ArrayList<Hero>();
  >         for (int i = 0; i < 10; i++) {
  >             heros.add(new Hero("hero " + i, r.nextInt(1000), r.nextInt(100)));
  >         }
  >         System.out.println("初始化后的集合：");
  >         System.out.println(heros);
  >         System.out.println("筛选出 hp>100 && damange<50的英雄");
  >         filter(heros);
  >     }
  >   
  >     private static void filter(List<Hero> heros) {
  >         for (Hero hero : heros) {
  >             if(hero.hp>100 && hero.damage<50)
  >                 System.out.print(hero);
  >         }
  >     }
  >   
  > }
  > ```
  >
  > 

* 匿名类

  > 代码实例
  >
  > ```java
  > package lambda;
  >    
  > import java.util.ArrayList;
  > import java.util.List;
  > import java.util.Random;
  >    
  > import charactor.Hero;
  >    
  > public class TestLambda {
  >     public static void main(String[] args) {
  >         Random r = new Random();
  >         List<Hero> heros = new ArrayList<Hero>();
  >         for (int i = 0; i < 5; i++) {
  >             heros.add(new Hero("hero " + i, r.nextInt(1000), r.nextInt(100)));
  >         }
  >         System.out.println("初始化后的集合：");
  >         System.out.println(heros);
  >         System.out.println("使用匿名类的方式，筛选出 hp>100 && damange<50的英雄");
  >         HeroChecker checker = new HeroChecker() {
  >             @Override
  >             public boolean test(Hero h) {
  >                 return (h.hp>100 && h.damage<50);
  >             }
  >         };
  >            
  >         filter(heros,checker);
  >     }
  >    
  >     private static void filter(List<Hero> heros,HeroChecker checker) {
  >         for (Hero hero : heros) {
  >             if(checker.test(hero))
  >                 System.out.print(hero);
  >         }
  >     }
  >    
  > }
  > ```
  >
  > 

* Lambal方式

  > 代码实例
  >
  > ```java
  > package lambda;
  >  
  > import java.util.ArrayList;
  > import java.util.List;
  > import java.util.Random;
  >  
  > import charactor.Hero;
  >  
  > public class TestLamdba {
  >     public static void main(String[] args) {
  >         Random r = new Random();
  >         List<Hero> heros = new ArrayList<Hero>();
  >         for (int i = 0; i < 5; i++) {
  >             heros.add(new Hero("hero " + i, r.nextInt(1000), r.nextInt(100)));
  >         }
  >         System.out.println("初始化后的集合：");
  >         System.out.println(heros);
  >         System.out.println("使用Lamdba的方式，筛选出 hp>100 && damange<50的英雄");
  >         filter(heros,h->h.hp>100 && h.damage<50);
  >     }
  >  
  >     private static void filter(List<Hero> heros,HeroChecker checker) {
  >         for (Hero hero : heros) {
  >             if(checker.test(hero))
  >                 System.out.print(hero);
  >         }
  >     }
  >  
  > }
  > ```
  >
  

### 线程interrupt

```java
package MultiThread.Chapter3;

import java.util.concurrent.TimeUnit;

public class ThreadInterrupt {
    public static void main(String[] args) throws InterruptedException {
        Thread thread=new Thread(()->{
            try {
                TimeUnit.MINUTES.sleep(1);
            } catch (InterruptedException e) {
                System.out.println("oh,i am be interrupt");
            }
        });
        thread.start();

        TimeUnit.MILLISECONDS.sleep(2);
        thread.interrupt();
    }
}

```

```java
package MultiThread.Chapter3;

import java.util.concurrent.TimeUnit;
//isInterrupt方法使用
public class ThreadisInterrupt {
    public static void main(String[] args) throws InterruptedException {
        Thread t1=new Thread()
        {
            @Override
            public void run() {
                while (true)
                {}
            }
        };
        //要设置在启动线程前
        t1.setDaemon(true);
        t1.start();

        TimeUnit.MILLISECONDS.sleep(2);
        System.out.println(t1.isInterrupted());
        t1.interrupt();
        System.out.println(t1.isInterrupted());
    }
}



//可中断方法会捕获中断信号，并且擦除interrupt标识
package MultiThread.Chapter3;

import java.sql.Time;
import java.util.concurrent.TimeUnit;

public class ThreadisInterrupted {
    public static void main(String[] args) throws InterruptedException {
        Thread t1=new Thread()
        {
            @Override
            public void run() {
                while(true)
                {
                    try {
                        TimeUnit.MINUTES.sleep(1);
                    } catch (InterruptedException e) {
                        System.out.println(isInterrupted());
                    }
                }
            }
        };
        t1.setDaemon(true);
        t1.start();
        TimeUnit.MILLISECONDS.sleep(2);
        System.out.printf("Thread is interrupt? %s\n",t1.isInterrupted());
        t1.interrupt();
        TimeUnit.MILLISECONDS.sleep(2);
        System.out.println(t1.isInterrupted());
    }
}
  
```

```java
package MultiThread.Chapter3;

import java.util.concurrent.TimeUnit;
//interrupted方法：判断是否interrupt,并且清除相关标志
public class Main {
    public static void main(String[] args) {
        System.out.printf("Main is interrupted? %s\n",Thread.interrupted());

        Thread.currentThread().interrupt();
        System.out.printf("Main is interrupted? %s\n",Thread.currentThread().isInterrupted());
        try {
            TimeUnit.MINUTES.sleep(2);
        } catch (InterruptedException e) {
            System.out.println("I will be interrupted still");        }
    }
}

```

### join方法

```java
package MultiThread.Chapter3;

import java.util.List;
import java.util.concurrent.TimeUnit;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class ThreadJoin {
    public static void main(String[] args) throws InterruptedException {

        List<Thread> threads=IntStream.range(1,3).mapToObj(ThreadJoin::create).collect(Collectors.toList());
        for(Thread thread:threads)
        {
            thread.start();
        }
        for(Thread thread:threads)
        {
            thread.join();
        }
        for(int i=0;i<10;i++)
        {
            System.out.println(Thread.currentThread().getName()+"#"+i);
            shortSleep();
        }
    }

    /**
     * 构建一个简单的线程
     * @param
     * @return
     */
    private static Thread create(int seq)
    {
        Thread thread= new Thread(() ->
        {
            for(int i=0;i<10;i++)
            {
                System.out.println(Thread.currentThread().getName()+"#"+i);
                shortSleep();
            }
        },String.valueOf(seq));
        return thread;
    }

    private static void shortSleep()
    {
        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

```

## Hook

* 获取线程运行时异常

  ```java
  package MultiThread.Chapter7;
  
  import java.util.concurrent.TimeUnit;
  
  //没有对thread指定handler的时候就去寻找默认的handler，如果默认的也没有，就去找group的uncaughtException方法。
  public class EmptyExceptionHandler {
  
      public static void main(String[] args) {
          ThreadGroup maingroup=Thread.currentThread().getThreadGroup();
          System.out.println(maingroup.getName());
          System.out.println(maingroup.getParent());
          System.out.println(maingroup.getParent().getParent());
          Thread thread=new Thread(maingroup,()->
          {
              try {
                  TimeUnit.SECONDS.sleep(2);
              } catch (InterruptedException e) {
                  e.printStackTrace();
              }
              System.out.println(1/0);
          },"test-thread");
          thread.start();
      }
  }
  
  ```

* 注入hook线程

  ```java
  package MultiThread.Chapter7;
  
  import java.util.concurrent.TimeUnit;
  
  public class ThreadHook {
      public static void main(String[] args) {
          //为应用程序注入钩子线程
          Runtime.getRuntime().addShutdownHook(new Thread()
          {
              @Override
              public void run() {
                  System.out.println("The hook thread 1 is running");
                  try {
                      TimeUnit.SECONDS.sleep(1);
                  } catch (InterruptedException e) {
                      e.printStackTrace();
                  }
                  System.out.println("The hook thread 1 will exit");
              }
          });
  
          Runtime.getRuntime().addShutdownHook(new Thread()
          {
              @Override
              public void run() {
                  System.out.println("The hook thread 2 is running");
                  try {
                      TimeUnit.SECONDS.sleep(1);
                  } catch (InterruptedException e) {
                      e.printStackTrace();
                  }
                  System.out.println("The hook thread 2 will exit");
              }
          });
          System.out.println("The program will is stoping");
      }
  }
  
  ```

* hook实战

  ```java
  package MultiThread.Chapter7;
  
  
  
  
  import java.io.IOException;
  import java.nio.file.Files;
  import java.nio.file.Path;
  import java.nio.file.Paths;
  import java.nio.file.attribute.PosixFilePermission;
  import java.nio.file.attribute.PosixFilePermissions;
  import java.util.Set;
  import java.util.concurrent.TimeUnit;
  
  public class PreventDuplicated {
      private final static String LOCK_PATH="";
      private final static String Lock_FILE="1.lock";
      private final static String PERMISSIONS="rw-------";
  
      public static void main(String[] args) throws IOException {
          //检查文件是否存在
          checkRunning();
          Runtime.getRuntime().addShutdownHook(new Thread(()->
          {
              System.out.println("The program received kill SIGNAL");
              getLockFile().toFile().delete();
          }));
          for(;;)
          {
              try {
                  TimeUnit.MILLISECONDS.sleep(1);
              } catch (InterruptedException e) {
                  e.printStackTrace();
              }
          }
  
      }
  
      private static void checkRunning() throws IOException {
          Path path=getLockFile();
          if(path.toFile().exists())
          {
              throw new RuntimeException("The program already running");
          }
  
          Files.createFile(path);
      }
      private static Path getLockFile()
      {
          return Paths.get(LOCK_PATH,Lock_FILE);
      }
  }
  
  ```

  
### 类的加载工程

* 类的主动使用

  * 通过new

  ```java
  //通过new进行初始化
  ```

  * 通过调用静态变量或者方法

  ```java
  //2.通过调用静态变量或者方法回到之类的初始化
  package MultiThread.Chapter9;
  
  public class Simple {
      static {
          System.out.println("I will be initialized");
      }
      public static int x=10;
  
      public static void test()
      {
          
      }
      public static void main(String[] args) {
          System.out.println(Simple.x);
      }
  }
  
  ```

  * 通过反射操作

  ```java
  //3.对某类进行反射操作，也会导致类的初始化
  ```

  * 通过初始化子类

  ```java
  //4.初始化子类会导致父类初始化。但是通过子类使用父类的静态变脸或者方法只会导致父类初始化
  ```

  * 通过执行main函数

  ```java
  //5.执行main函数所在的类会导致该类的初始化
  ```

* 类的被动使用

  * 构造某个类的数组时并不会导致初始化

  ```java
  package MultiThread.Chapter9;
  //被动使用不会导致类的加载和初始化
  public class ActiveLoadTest {
      public static void main(String[] args) {
         Simple []simples=new Simple[10];
          System.out.println(simples.length);
      }
  }
  
  ```

  * 引用类的静态常量的时候

  ```java
  public class GlobalConstants
  {
  public final static int MAX=1000;
  }
  ```

* 类的加载过程

  * 编译为class文件
  
  * 加载class文件进内存
  
  * 连接阶段
    * 验证
    * 准备
    * 解析
    
  * 初始化
  

### JVM类加载器

* JVM内置三大类加载器

  * 根类加载器

    ```java
    package MultiThread.Chapter10;
    
    public class BootstrapClassLoader {
        public static void main(String[] args) {
            System.out.println("Bootstrap:"+String.class.getClassLoader());
            System.out.println(System.getProperty("sun.boot.class.path"));
        }
    }
    
    ```

  * 扩展类加载器

    ```java
    package MultiThread.Chapter10;
    
    public class ExtClassLoader {
        public static void main(String[] args) {
            System.out.println(System.getProperty("java.ext.dirs"));
        }
    }
    
    ```

  * 系统类加载器

    ```java
    package MultiThread.Chapter10;
    
    public class ApplicationClassLoader {
        public static void main(String[] args) {
            System.out.println(System.getProperty("java.class.path"));
            System.out.println(ApplicationClassLoader.class.getClassLoader());
        }
    }
    
    ```

    

* 自定义类加载器

  ```java
  package MultiThread.Chapter10;
  
  import java.io.ByteArrayOutputStream;
  import java.io.IOException;
  import java.nio.file.Files;
  import java.nio.file.Path;
  import java.nio.file.Paths;
  
  public class MyClassLoader extends ClassLoader {
      private final static Path DEFAULT_CLASS_DIR=Paths.get("d:","classloader");
      private final Path classDir;
      //使用默认的class路径
      public MyClassLoader()
      {
          super();
          this.classDir=DEFAULT_CLASS_DIR;
      }
      //允许传入指定路径的class路径
      public MyClassLoader(String classDir)
      {
          super();
          this.classDir=Paths.get(classDir);
      }
  
      //指定class路径的同事，指定父类加载器
      public MyClassLoader(String classDir,ClassLoader parent)
      {
          super(parent);
          this.classDir=Paths.get(classDir);
      }
  
      @Override
      protected Class<?> findClass(String name) throws ClassNotFoundException {
          //读取class的二进制数据
          byte[] classbytes=this.readClassBytes(name);
          if(classbytes==null ||classbytes.length==0)
          {
              throw new ClassNotFoundException("Can not load the class"+ name);
          }
          //调用defineClass定义class
  
          return this.defineClass(name,classbytes,0,classbytes.length);
      }
  
      //将class文件读入内存
      private byte[] readClassBytes(String name) throws ClassNotFoundException {
          //将包名分隔符转换为文件路径分隔符
          String classPath=name.replace(".","/");
          Path classFullPath=classDir.resolve(Paths.get(classPath+".class"));
          if(!classFullPath.toFile().exists())
              throw new ClassNotFoundException("The class"+name+" not found.");
          try(ByteArrayOutputStream baos=new ByteArrayOutputStream())
          {
              Files.copy(classFullPath,baos);
              return baos.toByteArray();
          }catch (IOException e)
          {
              throw new ClassNotFoundException("Load the class"+ name+" occur error.",e);
          }
  
      }
  
      @Override
      public String toString() {
          return "My ClassLoader";
      }
  }
  
  ```

* 双亲委托机制

  * 工作流程

    >1.当前ClassLoader首先从自己已经加载的类中查询是否此类已经加载，如果已经加载则直接返回原来已经加载的类。
    >
    >每个类加载器都有自己的加载缓存，当一个类被加载了以后就会放入缓存，等下次加载的时候就可以直接返回了。
    >
    >2.当前classLoader的缓存中没有找到被加载的类的时候，委托父类加载器去加载，父类加载器采用同样的策略，首先查看自己的缓存，然后委托父类的父类去加载，一直到bootstrp ClassLoader.
    >
    >3.当所有的父类加载器都没有加载的时候，再由当前的类加载器加载，并将其放入它自己的缓存中，以便下次有加载请求的时候直接返回。

  * 破坏双委托机制
  
    ```java
    package MultiThread.Chapter10;
    
    import java.io.ByteArrayOutputStream;
    import java.io.IOException;
    import java.nio.file.Files;
    import java.nio.file.Path;
    import java.nio.file.Paths;
    
    public class BrokerDelegateClassLoader extends ClassLoader {
        private final static Path DEFAULT_CLASS_DIR=Paths.get("d:","classloader");
        private final Path classDir;
        //使用默认的class路径
        public BrokerDelegateClassLoader()
        {
            super();
            this.classDir=DEFAULT_CLASS_DIR;
        }
        //允许传入指定路径的class路径
        public BrokerDelegateClassLoader(String classDir)
        {
            super();
            this.classDir=Paths.get(classDir);
        }
    
        //指定class路径的同事，指定父类加载器
        public BrokerDelegateClassLoader(String classDir,ClassLoader parent)
        {
            super(parent);
            this.classDir=Paths.get(classDir);
        }
    
        @Override
        protected Class<?> findClass(String name) throws ClassNotFoundException {
            //读取class的二进制数据
            byte[] classbytes=this.readClassBytes(name);
            if(classbytes==null ||classbytes.length==0)
            {
                throw new ClassNotFoundException("Can not load the class"+ name);
            }
            //调用defineClass定义class
    
            return this.defineClass(name,classbytes,0,classbytes.length);
        }
    
        //将class文件读入内存
        private byte[] readClassBytes(String name) throws ClassNotFoundException {
            //将包名分隔符转换为文件路径分隔符
            String classPath=name.replace(".","/");
            Path classFullPath=classDir.resolve(Paths.get(classPath+".class"));
            if(!classFullPath.toFile().exists())
                throw new ClassNotFoundException("The class"+name+" not found.");
            try(ByteArrayOutputStream baos=new ByteArrayOutputStream())
            {
                Files.copy(classFullPath,baos);
                return baos.toByteArray();
            }catch (IOException e)
            {
                throw new ClassNotFoundException("Load the class"+ name+" occur error.",e);
            }
    
        }
    
        @Override
        protected Class<?> loadClass(String name, boolean resolve) throws ClassNotFoundException {
            //根据类的全路径名称进行加锁，确定每一个类再多线程中只被加载一次
            synchronized (getClassLoadingLock(name))
            {
                //到已加载类的缓存中查看该类是否也被加载，如果时则返回
                Class<?> klass=findLoadedClass(name);
                if(klass==null)
                {
                    //如果缓存中没有加载该类，并且是以java或者javax开头的类路径，则委托给系统类加载器进行加载
                    if(name.startsWith("java.") || name.startsWith("javax."))
                    {
                        try {
                            klass=getSystemClassLoader().loadClass(name);
                        }catch (Exception e)
                        {
    
                        }
                    }
                    //如果缓冲中没有，且暴不是以java或者javax开头的，则尝试委托给自定义的类加载器进行加载
                    else {
                        try {
                            klass=this.findClass(name);
                        }catch (ClassNotFoundException e)
                        {
    
                        }
                        //如果自定义加载器不能完成类的加载，则委托给父类加载器进行加载或者系统加载器进行加载
                        if(klass==null)
                        {
                            if(getParent()!=null)
                            {
                                klass=getParent().loadClass(name);
                            }
                            else
                            {
                                klass=getSystemClassLoader().loadClass(name);
                            }
                        }
                    }
                }
                //经过若干次尝试后，还是没有完成对类的加载，则抛出无法加载的异常
                if(null==klass)
                {
                    throw new ClassNotFoundException("The class "+name+" not found.");
                }
                if(resolve)
                {
                    resolveClass(klass);
                }
                return klass;
    
            }
        }
    
        @Override
        public String toString() {
            return "My ClassLoader";
        }
    }
    
    ```

* 类加载器命名空间、运行时包、类的卸载

  * 类加载器命名空间（每一个类加载器实例都有各自的命名空间）

    > 1.不同类加载器加载同一个class会产生不同对象
    >
    > 2.相同类加载器的不同实例加载同一个class也会产生不同对象
    >
    > **同一个class实例同一个类加载器命名空间之下才是唯一的**

  * 运行时包（由类加载的命名空间和类的全限定名称共同组成）
  
    > 比如Test运行时包如下所示：
    >
    > BootstrapClassLoader.ExtClassLoader.AppClassloader.MyClassLoader.com.wangwenjun.cuncurrent.chapter10.Test

  * 初始类加载器
  
    > 在类的加载工程中，所有参与的类加载器，即使没有亲自加载过该类，也都会标识为该类的初始类加载器。并且把该类放在该类加载维护的列表中。

  * 类的卸载
  
    > Class满足以下三个条件才会被GC回收：
    >
    > - 该类所有的实例都被GC，比如Simple.class的所有实例都被回收掉
    > - 加载该类的ClassLoader实例被回收
    > - 该类的class实例没有在其他地方被引用



### Volatile

* 初始Volatile关键词

  > 该关键词只能修饰类变量和实例变量，对于方法参数、局部变量以及实例常量，类常量都不能进行修饰。

* 并发编程的三个重要特性

  * 原子性

    > 一次的操作或者多次操作中，要么所有操作都执行，要吗都不执行
    >
    > volatile不保证数据的原子性，synchronized关键词保证

  * 有序性
  
    > 程序代码在执行过程中的先后顺序

  * 可见性
  
    > 当一个线程对共享变量进行了修改，那么另外的线程可以立即看到修改后的最新值。

* JMM如何保证三大特性

  * JMM与原子性（volatile不保证）

    > JMM只保证了基本读取和复制的原子性操作

  * JMM与可见性(volatile保证)
  
    >1.使用关键词volatile
    >
    >2.使用synchronized
    >
    >3.通过JUC提供的显示锁LOCK

  * JMM与有序性(volatile可保证)

* volatile的使用场景

  * 开关控制利用可见性的特点

  * 状态标记利用顺序性特点

  * Singleton设计模式的double-check也是利用了顺序性特点

* volatile与synchronized的区别

  * 使用上的区别

    > 1.volatile关键词只能用于修饰实列变量或者类变量，不能用于修饰方法以及方法参数和局部变量、常量
    >
    > 2.synchronized关键词不能用于对变量的修饰，只能用于方法和语句块
    >
    > 3.volatile修饰的变量可以是null，synchronized关键词同步语句块的monitor对象不能为null
    
  * 对原子性的保证
  
    > 1.volatile无法保证原子性
    >
    > 2.synchronized是一种排他机制，所以synchronized可以保证原子性
  
  * 对可见性的保证
  
    > 1.两者均可以保证共享资源在多线程间的可见性，但是实现机制完全不同
    >
    > 2.synchronized借助于JVM指令的monitor enter和monitor exit通过排他的方式使得同步代码串行化，在monitor exit时所有共享资源都会刷星到主内存中

  * 对有序性的保证
  
    > 1.volatile关键词禁止JVM编译器以及处理器对其进行重排序
    >
    > 2.synchronized对于所修饰的代码块不能保证有序性

  * 其他
    > 1.volatile不会使线程陷入阻塞
    > 2.synchronized会使线程进入阻塞状态



### 锁的概述

* 乐观锁和悲观锁

  * 悲观锁

    > 排他性

  * 乐观锁
    
    > 更新的时候具有排他性
* 公平锁与非公平锁
  * 公平锁
    
    > 按照线程请求锁的时间早晚来决定
    >
    > ```java
    > ReentrantLock pairLock=new ReentrantLock(true)
    > ```
  * 非公平锁
    
    > ```java
    > ReentrantLock pairLock=new ReentrantLock(false)
    > ```
    >
    > 在没有公平性需求的前提下尽量使用非公平锁，因为公平锁会带来性能开销 。
    >

* 独占锁与共享锁

  * 独占锁

    > 保证任何时候都只有一个线程能得到锁。ReentrantLock就是以独占方式实现

  * 共享锁
    
    > 可以同时由多个线程持有。ReadWriteLock读写锁就是



* 可重入锁

  > 当一个线程要获取一个被其他线程持有的独占锁时，该线程会被阻塞，那么当一个线程再次获取它自己己经获取的锁时是否会被阻塞呢？如果不被阻塞，那么我们说该锁是可重入的，也就是只要该线程获取了该锁，那么可以无限次数（在高级篇中我们将知道，严格来说是有限次数）地进入被该锁锁住的代码 。 

* 自旋锁

  > 如果发现锁已经被其他线程占有，它不马上阻塞自己，在不放弃 CPU 使用权的情况下，多次尝试获取（默认次数是 10，可以使用 －XX :PreBlockSpinsh 参数设置该值），很有可能在后面几次尝试中其他线程己经释放了锁 。 如果尝试指定的次数后仍没有获取到锁则当前线程才会被阻塞挂起 。  



### ThreadLocalRandom

* random

  > 对于随机中子数采用的是原子性变量，所以在更新中子数的时候只有一个线程可以进行，其他线程自旋，性能低

* ThreadLocalRandom

  ```java
  //类似于
  package com.zjs.Chapter1.Chapter3;
  
  import java.util.Random;
  import java.util.concurrent.ThreadLocalRandom;
  
  public class RandomTest {
     // static final ThreadLocalRandom random=ThreadLocalRandom.current();
      public static void main(String[] args) {
  
          for(int i=0;i<10;i++)
          {
              new Thread()
              {
                  @Override
                  public void run() {
                      System.out.println(Thread.currentThread().getName()+": "+ThreadLocalRandom.current().nextInt(100));
                  }
              }.start();
          }
      }
  }
  
  ```

  



### CAS操作





## 设计模式

### 七种单例设计模式

* 饿汉式

  ```java
  package com.zjs.Chapter1.Chapter14;
  
  public final class SingLeton {
      //实列变量
      private byte[] data=new byte[1024];
      private static SingLeton singLeton=new SingLeton();
      private SingLeton()
      {}
  
      public SingLeton getSingLeton()
      {
          return singLeton;
      }
  }
  
  ```

* 懒汉式

  ```java
  package com.zjs.Chapter1.Chapter14;
  //在多线程的情况下不能保证单例的唯一性
  public class LazySingLeton {
      private byte[] data=new byte[1024];
      private static LazySingLeton singLeton=null;
      private LazySingLeton()
      {}
      public static LazySingLeton getLazySingLeton()
      {
          if(singLeton==null) {
              singLeton=new LazySingLeton();
          }
          return singLeton;
      }
  }
  
  ```

* 懒汉式+同步方法

  ```java
  package com.zjs.Chapter1.Chapter14;
  
  public class LazySingLeton {
      private byte[] data=new byte[1024];
      private static LazySingLeton singLeton=null;
      private LazySingLeton()
      {}
      //加入Synchronized方法同步控制，每次只能有一个线程能够进入,但是性能低下
      public static synchronized LazySingLeton getLazySingLeton()
      {
          if(singLeton==null) {
              singLeton=new LazySingLeton();
          }
          return singLeton;
      }
  }
  
  ```

* double-check

  ```java
  package com.zjs.Chapter1.Chapter14;
  
  import java.net.Socket;
  import java.sql.Connection;
  
  public class Double_Check {
      private byte[] data=new byte[1024];
      private static Double_Check double_check=null;
      Connection conn;
      Socket socket;
      private Double_Check()
      {
          this.conn;
          this.socket  //初始化
      }
  
      public static Double_Check getInstance()
      {
          if(double_check==null)
          {
              synchronized (Double_Check.class)
              {
                  if(double_check==null)
                  {
                      double_check=new Double_Check();
                  }
  
              }
          }
          return double_check;
      }
  
  
  }
  
  ```

* volatile+checked-check

  ```java
  //加上volatile
  private volatile static SingLeton instance=null;
  ```

* holder单例模式

  ```java
  package SingleInstanceModel;
  /**
   * 在Singleton_4类中并没有instance的静态成员，而是将其放到了静态内部类Holder之中，因此在Singleton类的初始化过程中并不会创建
   * Singleton的实例，Holder类中定义了Singleton的静态变量，并且直接进行了实例化，当Holder被主动引用的时候则会创建Singleton的实例，
   * Singleton实例的创建过程在Java程序编译时期收集至<clinit>()方法中，该方法又是同步方法，同步方法可以保证内存的可见性、JVM指令的顺序性和原子性
   * Holder方式的单例设计是最好的设计之一，也是目前使用比较广的设计之一。
   * */
  
  /**
   * Created by JYM on 2019/1/8	
   * 单例模式：Holder方式
   * Holder的方式完全是借助了类加载的特点。
   * */
  
  //final 不允许被继承
  public final class Singleton_4
  {
      //实例变量
      private byte[] data = new byte[1024];
  
      private Singleton_4()
      {}
  
      //在静态内部类中持有Singleton的实例，并且可被直接初始化；
      private static class Holder
      {
          private static Singleton_4 instance = new Singleton_4();
      }
  
      //调用getInstance方法，事实上是获得Holder的instance静态属性
      public static Singleton_4 getInstance()
      {
          return Holder.instance;
      }
  }
  
  ```

* Enum单例模式

  ```java
  package com.zjs.Chapter1.Chapter14;
  
  public enum  EnumSingLeton {
      INSTANCE;
      private byte[] data=new byte[1024];
      EnumSingLeton()
      {
          System.out.println("INSTANCE will be initialized immdiately");
      }
      public static void methode()
      {
          //调用该方法则会主动使用EnumSingLeton,INSTANCE将会被实列化
      }
      public static EnumSingLeton getInstance()
      {
          return INSTANCE;
      }
  
  }
  
  ```

  
### 监控任务的生命周期

* 观察者模式

  ```java
  Observable接口
  public interface Observable {
      enum Cycle{
          STARTED,RUNNING,DONE,ERROR
      }
      Cycle getCycle();
      void start();
      void interrupt();
  }
  
  //该接口用于返回线程的状态。其中枚举类定义了线程的四个状态。start()方法与interrupt()方法用于屏蔽Thread类其他API。
  
  //TaskLifecycle接口
  public interface TaskLifecycle<T> {
      void onStart(Thread thread);
      void onRunning(Thread thread);
      void onFinish(Thread thread ,T result);
      void onError(Thread thread,Exception e);
      class EmptyLifecycle<T> implements TaskLifecycle<T>{
  
          @Override
          public void onStart(Thread thread) {
          }
  
          @Override
          public void onRunning(Thread thread) {
          }
  
          @Override
          public void onFinish(Thread thread, T result) {
          }
  
          @Override
          public void onError(Thread thread, Exception e) {
          }
      }
  }
  //该接口定了了几个在线程不同状态下会被运行的方法。
  
  //Task接口
  public interface Task<T> {
      T call();
  
  //相当于任务类，会被线程所执行。
  
  //实现ObservableThread
  public class ObservableThread<T> extends Thread implements Observable{
      private  final  TaskLifecycle lifecycle;
      private  final  Task task;//任务
      private  Cycle cycle;
      //构造方法
      public ObservableThread(Task<T> task){
          this(new TaskLifecycle.EmptyLifecycle(),task);
      }
      //构造方法
      public ObservableThread(TaskLifecycle lifecycle, Task<T> task) {
          super();
          if(task==null)
              throw  new IllegalArgumentException("The task is required.");
          this.lifecycle = lifecycle;
          this.task = task;
      }
      //重写run方法
      public final void run(){
      	//先是STARTED状态
          this.update(Cycle.STARTED,null,null);
          try{
          	//RUNNING状态
              this.update(Cycle.RUNNING,null,null);
              //执行call方法
              T result = (T) this.task.call();
              //DONE状态
              this.update(Cycle.DONE,result,null);
          }catch (Exception e){
          //ERROR状态
              this.update(Cycle.ERROR,null,e);
          }
      }
      @Override
  	//得到线程状态
      public Cycle getCycle() {
          return this.cycle;
      }
      //根据线程不同状态来调用TaskLifecycle中定义的方法
      private  void update(Cycle cycle, T result,Exception e){
          this.cycle=cycle;
          if(lifecycle==null){
              return ;
          }
          try{
              switch (cycle){
                  case STARTED:
                      this.lifecycle.onStart(currentThread());break;
                  case RUNNING:
                      this.lifecycle.onRunning(currentThread());break;
                  case DONE:
                      this.lifecycle.onFinish(currentThread(),result);break;
                  case ERROR:
                      this.lifecycle.onError(currentThread(),e);break;
              }
          }catch(Exception ex){
              if(cycle==Cycle.ERROR){
                  throw  ex;
              }
          }
      }
  }
  
  //run方法会在线程不同的时刻(Observable 提供)通过update方法来调用TaskLifecycle中定义好的方法。并且会在run方法中执行定义的任务接口中的方法Task();
  
  //测试
  public class test {
      public static void main(String[] args) {
          //实现一个TaskLifecycle接口 并且重写方法，被线程在不同状态下调用
          final TaskLifecycle<Integer> lifecycle = new TaskLifecycle.EmptyLifecycle<Integer>() {
              @Override
              public void onStart(Thread thread) {
                  System.out.println("线程准备运行");
              }
  
              @Override
              public void onRunning(Thread thread) {
                  System.out.println("线程正在运行");
              }
  
              @Override
              public void onFinish(Thread thread, Integer result) {
                  System.out.println("线程运行完成 结果为:"+result);
              }
  
              @Override
              public void onError(Thread thread, Exception e) {
                  System.out.println("线程出错");
              }
          };
          
         Observable o =new ObservableThread<>(lifecycle,()->{
             int a=0;
             for(int i=0;i<10;i++){
                 a+=i;
             }
             return a;
         });
         o.start();
      }
  }
  
  ```


### JDK8新增LongAdder和LongAccumulator

```Java
//LongAdder是LongAccumulator的特列，后者相比前者来说，可以自定义累加规则以及指定初始化值,前者初始化值为0。两者皆采用CAS非阻塞方式。
package com.zjs.Chapter4;

import java.util.concurrent.atomic.LongAccumulator;
import java.util.function.LongBinaryOperator;

public class LongAccumulator1{
    private static LongAccumulator instance = new LongAccumulator(new LongBinaryOperator(){
        @Override
        public long applyAsLong(long left, long right) {
            return (left + right)*2;
        }
    },1);

    public static void main(String[] args) throws InterruptedException {
        Thread t2 = new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i=0;i<5;i++) {
                    instance.accumulate(1);
                    System.out.println("第"+i+"次的值为"+instance.get());
                }
            }
        }
        );

        t2.start();
        t2.join();
        System.out.println(instance.get());
    }
}

```



### Java并发包中并发List源码剖析

* CopyOnWriteArrayList

  * 介绍

    > CopyOnWriteArrayList 是无界 （list），并把新增的元素添加到新数组。在添加元素时，首先复制了 一个快照，然后在快照上进行添加，而不是直接在原来数组上进行。  
    
  * 弱一致性的迭代器
  
    > 其他线程的操作对迭代器是不可见的，迭代器的初始化要在子线程操作之前
  



### LockSupport工具类

* park()

  ```java
  package com.zjs.Chapter6;
  
  import java.util.concurrent.locks.LockSupport;
  //如果当前线程没有与LockSupport相关的许可证，则会阻塞,且该阻塞
  public class Test1 {
      public static void main(String[] args) {
          System.out.println("begin park");
          LockSupport.park();
          System.out.println("end park");
      }
  }
  
  ```

* unpark()

  ```java
  package com.zjs.Chapter6;
  
  import java.util.concurrent.locks.LockSupport;
  
  public class Test1 {
      public static void main(String[] args) {
          System.out.println("begin park");
          //传入参数的线程没有许可证，则会让他拥有许可证，如果因为park阻塞则会唤醒。如果没有调用park,则下次该线程调用park的时候会立刻返回
          LockSupport.unpark(Thread.currentThread());
          LockSupport.park();
          System.out.println("end park");
      }
  }
  
  ```

  

