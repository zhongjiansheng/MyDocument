## Chapter1 

#### 1 UML

UML图用实心菱形表明组合关系。其中强联系的为指向的地方。

UML图用空心三角表明继承关系，其中父类为指向的地方

## Chapter2

#### 1 成员变量和局部变量的区别

1. 局部变量必须初始化，成员变量可以不用（**不推荐**）
2. 局部变量不能使用**权限修饰符**进行修饰，但可以使用**final**
3. 局部变量周期是随着方法，成员变量是随着实例
4. 成员变量放在堆当中，局部变量在栈当中

#### 2 static

该关键词声明的属性和方法属于类拥有，与实例无关，且方法不能访问非静态的方法或者属性，因为不能进行实例化。

## Chapter3

#### 1 equal()

该方法默认是比较的引用，如果要自定义比较方式，需要进行重写

#### 2 && and ||

该逻辑表达式为短路，只要其中一个不满足，则后面的都不会执行

#### 3 sizeof

Java当中没有**sizeof**操作符，因为所有数据类型在所有机器中的大小是相同的

## Chapter4

#### 1 while(true)和for(;;)

两者都是无限循环，在优化的编译器中指令一样，但在没有优化的编译器中，**for(;;)**的指令更少

#### 2 带标签的Break\Continue

```java
public static void main(String[] args) {
        int i = 0;
        outer:
        while (true) {
            System.out.println("outer while loop");
            while (true) {
                i++;
                System.out.println("i=" + i);
                if (i == 1) {
                    System.out.println("continue");
                    continue ;
                }
                if (i == 3) {
                    System.out.println("continue outer");
                    continue outer;
                }
                if (i == 5) {
                    System.out.println("break");
                    break ;
                }
                // 中断并跳出标签所指的循环
                if(i == 7) {
                    System.out.println("break outer");
                    break outer;
                }
            }
        }
    }
```

## Chapter5

#### 1 重载

重载是类部进行，利用参数列表进行区分。

#### 2 基本类型的重载

类型小了，没匹配到方法，就将类型变大去匹配。如果类型大了，就缩小去匹配。

#### 3 this

this调用构造函数的时候，只能放在最前面，且只能调用一次

#### 4 初始化的条件

> 1.调用类的静态方法或者变量的时候
>
> 2.采用反射创建类的时候
>
> 3.main()方法执行的时候 

## Chapter6

#### 1 private、protected、public、无修饰符

>private:只允许类内可访问
>
>protected:包内任何类都可以访问，继承类在任何包可访问
>
>无修饰符:包内任何类都可以访问
>
>public:没有限制

## Chapter7

#### 1 final关键词

修饰基本类型表示不能改变、修饰非基本类型表示其引用不能改变，但引用对象的内容可以改变。

## Chapter9

#### 1 extends

如果继承的是实现类，只能单继承，如果是接口，可以多继承

## Chapter10

#### 1 匿名内部类

 使用匿名内部类有个前提条件：必须继承一个父类或实现一个接口 

## Chapter12

#### 1 异常链

通过异常链可以在抛出新的异常的时候保存原始异常，只有**Error**、**Exception**、**RuntimeException**三种提供了带cause参数的构造器。

#### 2 Java标准异常

* RuntimeException

  > 不受检查异常，运行时候回自动捕获，不用自己去捕获，但是可以在代码中抛出RuntimeException

#### 3 异常匹配

找到匹配的处理程序之后，它就认为异常将得到处理，然后就不再继续查找。

## Chapter13

#### 1 String不变性

string作为参数传递的是引用，只提供只读，不会改变数据。