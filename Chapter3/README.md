# 第三章:Java

#  *中级知识*

## Java多线程编程

### Java多线程技能

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

### 线程优先级

* setPriority()：设置线程级别
* 线程可以继承，且继承的级别一样
* 线程优先级具有不确定性和随机性



## Lambal

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
  > 



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