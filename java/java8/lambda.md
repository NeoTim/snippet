# Lambda 和 Stream 示例

<!-- toc -->

## 继承单方法接口
对于只有一个抽象方法的接口，可以使用 lambda 表达式代替匿名类
```java
new Thread( () -> System.out.println("In Java8, Lambda expression rocks !!") ).start();
```
```java
JButton show =  new JButton("Show");
show.addActionListener((e) -> { System.out.println("Light, Camera, Action !! Lambda expressions Rocks"); });
```

## 遍历 List
```java
List features = Arrays.asList("Lambdas", "Default Method", "Stream API", "Date and Time API");
features.forEach(n -> System.out.println(n));
// 使用方法引用
features.forEach(System.out::println);
```

## 使用 Predicate 接口
```java
public static void main(String args[]) {
    List languages = Arrays.asList("Java", "Scala", "C++", "Haskell", "Lisp");
    System.out.println("Languages which starts with J :");
    filter(languages, (str) -> str.startsWith("J"));
    System.out.println("Languages which ends with a ");
    filter(languages, (str) -> str.endsWith("a"));
    System.out.println("Print all languages :");
    filter(languages, (str) -> true);
    System.out.println("Print no language : ");
    filter(languages, (str) -> false);
    System.out.println("Print language whose length greater than 4:");
    filter(languages, (str) -> str.length() > 4);
}

public static void filter(List<String> names, Predicate<String> condition) {
    for (String name : names) {
        if (condition.test(name)) {
            System.out.println(name + " ");
        }
    }
}

//Even better
public static void filter(List<String> names, Predicate<String> condition) {
    names.stream().filter((name) -> (condition.test(name)))
            .forEach((name) -> {
                        System.out.println(name + " ");
                    }
            );
}

```
使用多个 Predicate
```java
// We can even combine Predicate using and(), or() And xor() logical functions
 // for example to find names, which starts with J and four letters long, you
 // can pass combination of two Predicate
 Predicate<String> startsWithJ = (n) -> n.startsWith("J");
 Predicate<String> fourLetterLong = (n) -> n.length() == 4;

 names.stream()
      .filter(startsWithJ.and(fourLetterLong))
      .forEach((n) -> System.out.print("\nName, which starts with 'J' and four letter long is : " + n));
```
## Map and reduce
```java
List costBeforeTax = Arrays.asList(100, 200, 300, 400, 500);
// 元素值变为原来的 1.12 倍
costBeforeTax.stream().map((cost) -> cost + .12*cost).forEach(System.out::println);
```
```java
List costBeforeTax = Arrays.asList(100, 200, 300, 400, 500);
// 元素值变为原来的1.2倍，并求和
double bill = costBeforeTax.stream().map((cost) -> cost + .12*cost)
                                    .reduce((sum, cost) -> sum + cost).get();
System.out.println("Total : " + bill);
```
## 最大、最小、平均、和
```java
//Get count, min, max, sum, and average for numbers
List<Integer> primes = Arrays.asList(2, 3, 5, 7, 11, 13, 17, 19, 23, 29);
IntSummaryStatistics stats = primes.stream().mapToInt((x) -> x).summaryStatistics();
System.out.println("Highest prime number in List : " + stats.getMax());
System.out.println("Lowest prime number in List : " + stats.getMin());
System.out.println("Sum of all prime numbers : " + stats.getSum());
System.out.println("Average of all prime numbers : " + stats.getAverage());
```
## 参考
* [10 Example of Lambda Expressions and Streams in Java 8](http://javarevisited.blogspot.com/2014/02/10-example-of-lambda-expressions-in-java8.html)
