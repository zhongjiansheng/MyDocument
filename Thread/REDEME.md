## 基础知识

1. 基础知识

   * 并发编程的优缺点

     * 为什么要用到并发？（优点）

     * 并发编程的缺点

       > 1.上下文切换耗费时效
       >
       > 2.容易受到JMM的影响

     * 易混淆的概念

2. 线程的状态和基本操作

   * 线程的创建

     * Thread
     * Runnable
     * Future\FutureTask

   * 线程通知与等待

     * wait()/notify()

       1.该函数Object所拥有，当一个线程调用共享变量的wait()方法的时候会阻塞，需等待其他线程调用该共享变量的notify()或者该阻塞线程的interrupt()方法才能退出阻塞.

       2.wait()只会释放当前的共享变量

       > 注意：wait()和sleep()的区别，wait的线程唤醒后还需要争夺资源，而sleep的则不会.

   * 线程状态的转换

     <img src="C:\Users\87360\AppData\Roaming\Typora\typora-user-images\1571209893029.png" alt="1571209893029"  />
   
   * 线程的基本操作
   
     * interrupted
   
     * join
   
     * sleep

       sleep()不会让出监视器锁
     
       > sleep()与wait()的区别：
       >
       > 1.sleep()是Thread的静态方法，wait()是Object实例方法
       >
       > 2.wait()必须在同步方法或同步块使用，也就是必须获得对象锁。而Sleep只会让出cpu资源，并不会释放对象锁。
       >
       > 3.sleep()方法在时间到后再次获取CPU时间片，而wait()方法必须等待Object.nodify()通知，且再次获得时间片才会运行。
      * yield
     
        **sleep**与**yield**方法的区别在于，当线程调用sleep 方法时调用线程会被阻塞挂起指定的时间，在这期间线程调度器不会去调度该线程。而调用yield方法时，线程只是让出自己剩余的时间片，并没有被阻塞挂起，而是处于就绪状态，线程调度器下一次调度时就有可能调度到当前线程执行
     
   * 守护线程Daemon
   
3. 死锁条件：

   * 互斥条件

     ==资源==只能被==一个线程==占有

   * 请求并持有条件

     一个线程持有了==至少一个资源==，并对其他线程==持有资源==做出请求

   * 不可剥夺条件

     线程获取到的资源在使用期间==不能被其他线程抢占==

   * 环路等待条件

     死锁线程并存在==一个资源的环形链==

4. 锁的分类

   * 悲观锁、乐观锁
   * 公平锁、非公平锁
   * 独占锁、共享锁
   * 自旋锁(CAS)

5. 并发理论（JMM）

6. 并发关键词

   * 实现原理

     ![1571212870810](C:\Users\87360\AppData\Roaming\Typora\typora-user-images\1571212870810.png)

   * CAS操作

     一种乐观锁策略

     * CAS应用场景

       JUC包中许多类都是使用的CAS。

     * CAS的问题

       1. ABA问题

          通过添加一个版本号解决（AtomicStampedReference）

       2. 自旋时间过长

       3. 只能保证一个共享变量的原子操作

          atomic中提供了AtomicReference来保证引用对象之间的原子性。


## Lock体系

##### 初识Lock与AbstractQueuedSynchronizer(AQS)

