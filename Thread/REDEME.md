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

   * 线程状态的转换

     ![1571209893029](C:\Users\87360\AppData\Roaming\Typora\typora-user-images\1571209893029.png)

   * 线程的基本操作

     * interrupted

     * join

     * sleep

       > sleep()与wait()的区别：
       >
       > 1.sleep()是Thread的静态方法，wait()是Object实例方法
       >
       > 2.wait()必须在同步方法或同步块使用，也就是必须获得对象锁。而Sleep只会让出cpu资源，并不会释放对象锁。
       >
       > 3.sleep()方法在时间到后再次获取CPU时间片，而wait()方法必须等待Object.nodify()通知，且再次获得时间片才会运行。
       >
       > 

      * yield
     
   * 守护线程Daemon
   
3. 并发理论（JMM）
   
4. 并发关键词

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

