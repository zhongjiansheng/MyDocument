# 第三章:Java

#  *中级知识*

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

## 网络编程

* 网络编程、IP地址、端口

  1. IP地址：32位，4个字节

  2. 端口：计算机之间通过端口进行通信

  3. 获取本机地址

     > 代码实例
     >
     > ```java
     >  InetAddress inetAddress=InetAddress.getLocalHost();
     >  System.out.println(inetAddress.getHostName());
     > ```
     >
     > 

* 网络编程Socket