# 第三章:Java

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

