# Strings

<!-- toc -->
## Joiner
```java
Joiner joiner = Joiner.on("; ").skipNulls();
return joiner.join("Harry", null, "Ron", "Hermione");

Joiner.on(",").join(Arrays.asList(1, 5, 7)); // returns "1,5,7"
```

## Splitter
```java
Splitter.on(',')
    .trimResults()
    .omitEmptyStrings()
    .split("foo,bar,,   qux");
```
