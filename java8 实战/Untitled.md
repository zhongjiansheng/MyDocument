## Chapter2

#### 1 行为参数化

可以利用以下三种方式实现：类、匿名类、Lambda

> 行为参数化：就是一个方法接受多个不同的行为作为参数，并在内部使用它们，完成不同行为的能力

## Chapter3

#### 1 Lambda的组成

参数列表、函数主题、返回类型、异常列表



#### 2 函数式接口

只定义一个抽象方法的接口。用**@FunctionalInterface**标注

#### 3 类型检查

* 首先找出filter方法的声明
* 要求是predicate<apple>目标对象的第二个正是参数
* predicate是一个函数式接口，定义了一个抽象方法
* 该抽象方法描述了一个函数描述符
* filter的任何实际参数必须匹配这个要求

#### 4 局部变量

Lambda表达式可以使用主体里面参数，但是捕获的参数会设置为final

#### 5 谓词复合

谓词接口:negate、and、or

* negate

  返回比较器的非

* and

  连接两个Lambda

* or

  连接两个Lambda,表示或

#### 6 函数复合

复合词：andThen、Compose

#### 7 Lambda小结

* Lambda没有名称，但有参数列表、函数主题、返回类型，可能还有一个可以抛出的异常的列表
* 函数式接口就是仅仅声明了一个抽象方法的接口
* 只有再接受函数式接口的地方才可以使用Lambda表达式

## Chapter4

#### 1 流只能遍历一次

#### 2 外部迭代与内部迭代

* for-each

  ```java
  List<String> names = new ArrayList<>();
  for(Dish d : menu) {
     names.add(d.getName())
  }
  ```

* Iterator

  ```java
  List<String> names = new ArrayList<>();
  Iterator<String> iter = menu.iterator();
  while(iter.hasNext()) {
       Dish d = iter.next();
       names.add(d.getName())
  }
  ```

* 流迭代

  ```java
  List<String> names = menu.stream().map(Dish::getName).collect(toList())
  ```

#### 3 流的使用

* 数据源：用来执行一个查询
* 中间操作链：形成一条流的流水线
* 终端操作：执行流水线，并能生成结果

## Chapter5

#### 1 数值范围

* rangeClosed

  ```java
   // 包含结束值
          IntStream evenNumbers = IntStream.rangeClosed(1,100).filter(i -> i % 2 == 0);
          System.out.println(evenNumbers.count());
  ```

* range

  不包含结束值

#### 2 创建流的方法

* 集合

  ```java
  Collection.stream()
  ```

* 数值范围创建

  range、rangeClosed

* 值创建

  ```java
   // 值创建流
          Stream<String> stream1 = Stream.of("java 8", "lambda", "in", "action");
          stream1.map(String::toUpperCase).forEach(System.out::println);
          // 获得一个空流
          Stream<String> stream2 = Stream.empty();
      }
  ```

* 由数组生成

  ```java
  // 由数组创建流
          int [] numbers = {2, 3, 5, 7, 11, 13};
          int sum = Arrays.stream(numbers).sum();
  ```

* 由文件生成

  ```java
    // 由文件生成流
          long uniqueWords = 0;
          try (Stream<String> lines = Files.lines(Paths.get("data.txt"), Charset.defaultCharset())){
              uniqueWords = lines.flatMap(d -> Arrays.stream(d.split(" "))).distinct().count();
          } catch (IOException e){
  
          }
  ```

* 由函数生成

  * 迭代

    ```java
      // 斐波纳契元组序列
            Stream.iterate(new int[]{0,1},c -> new int[]{c[1],c[0]+c[1]}).limit(20).forEach(c -> System.out.println(c[0] + "," + c[1]));
        }
    ```

  * 生成

    ```java
    Stream.generate(Math::random)
    .limit(5)
    .forEach(System.out::println);
    
    ```

## Chapter6

#### 1 归约与汇总

* 流的最大值和最小值

  ```java
     long howManyDishes = menu.stream().collect(counting());
          System.out.println("菜单里面菜的数量为:" + howManyDishes);
          // 查找流的最大值和最小值
          Optional<Dish> mostCalorieDish = menu.stream().collect(maxBy(Comparator.comparing(Dish::getCalories)));
          System.out.println("菜单里面菜最贵的为:" + mostCalorieDish);
  ```

* 汇总

  ```java
    int totalCalories = menu.stream().collect(summingInt(Dish::getCalories));
          System.out.println("菜单上菜品的热量为:" + totalCalories);
  
          // 返回所有的综合信息
          IntSummaryStatistics menuStatistics = menu.stream().collect(summarizingInt(Dish::getCalories));
          System.out.println(menuStatistics);
  ```

#### 2 分组

```java
//groupingBy后面可以再接groupingBy或者其他函数
Map<Dish.Type, List<Dish>> dishesByType =
menu.stream().collect(groupingBy(Dish::getType));

```

* 转换类型

  使用**Collectors.collectingAndThen()**可以转换类型

  ```java
  Map<Dish.Type, Dish> mostCaloricByType =
  menu.stream()
  .collect(groupingBy(Dish::getType,
  collectingAndThen(
  maxBy(comparingInt(Dish::getCalories)),
  Optional::get)));
  
  
  ```

#### 3 分区

分区和分组类似，只不过一个是二分类，一个是多分类

```java
        // 分区
        Map<Boolean,List<Dish>> partitionedMenu = menu.stream().collect(partitioningBy(Dish::isVegetarian));
        System.out.println(partitionedMenu);

```









