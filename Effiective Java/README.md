## 条目1.使用静态工厂方法替代构造方法

在类当中提供一个公共静态工厂方法，只返回类实例。

```java
public static Boolean valueOf(boolean b)
{
    return b? Boolean.TRUE : Boolean.FALSE
}
```

静态工厂方法又有**优点也有缺点**

1. 优点

   * 拥有自己的名字，方便分类

   * 不需要每次调用时都创建一个新对象

   * 可以返回其返回类型的任何子类型的对象

   * 返回对象的类可以根据输入参数的不同而不同

   * 在编写包含该方法的类时，返回的对象的类不需要存在

     > 参考服务提供者模式

2. 缺点

   * 如果该类没有公共或受保护构造方法的类不能被**子类化**

     ```java
     //以下代码会报错
     public class SumDemo{
        private SumDemo()
        {}
     }
     public class Demo extend SumDemo
     {
     
     }
     ```

   * 在API文档中很难发现



##  条目2.当构造方法参数过多时使用builder模式

 ```java
package effective_java;

/**
 * @author : zhongjiansheng
 * @date : 2019/9/3 16:52
 */
public class T2 {
    private final int servingSize;
    private final int servings;
    private final int calories;
    private final int fat;
    private final int sodium;
    private final int carbohydrate;

    public static class Builder {
        //required parameters
        private final int servingSize;
        private final int servings;

        private int calories = 0;
        private int fat = 0;
        private int sodium = 0;
        private int carbohydrate = 0;

        public Builder(int servingSize, int servings) {
            this.servingSize = servingSize;
            this.servings = servings;
        }

        public Builder calories(int colories) {
            this.calories = colories;
            return this;
        }

        public Builder fat(int fat) {
            this.fat = fat;
            return this;
        }
        public Builder sodium(int sodium)
        {
            this.sodium=sodium;
            return this;
        }
        public Builder carbohydrate(int carbohydrate)
        {
            this.carbohydrate=carbohydrate;
            return this;
        }

        public T2 create(Builder builder) {
            return new T2(builder);
        }
    }
    private T2(Builder builder)
    {
        this.calories=builder.calories;
        this.carbohydrate=builder.carbohydrate;
        this.fat=builder.fat;
        this.servings=builder.servings;
        this.servingSize=builder.servingSize;
        this.sodium=builder.sodium;
    }
}

 ```



