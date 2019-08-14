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