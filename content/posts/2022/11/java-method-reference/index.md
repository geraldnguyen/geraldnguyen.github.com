---
title: "Java Method Reference"
subtitle: Method reference lets you use an existing method as a lambda as long as their signature (input & output) are compatible.
image: 1_ypHleI7FEQsx3ZrsTQBvlQ.png
date: 2022-11-27T09:19:42+01:00
categories: [Software Development]
tags: [Java, Functional Interface, Method Reference]
medium: https://medium.com/codex/java-method-reference-7b4f260bc3ad
---

I first learned method reference from C#, so the Java concept is familiar to me. Or that’s what I thought until I picked up the [Modern Java Recipes book](https://www.oreilly.com/library/view/modern-java-recipes/9781491973165/).

In this article, we’ll explore 3 forms of method references in Java

*   `object::instanceMethod`
*   `Class::staticMethod`
*   `Class::instanceMethod`

The last one is a bit more special, as we shall examine in detail

{{< figure src="1_ypHleI7FEQsx3ZrsTQBvlQ.png" caption="Source: author" >}}


# object::instanceMethod

The `object::instanceMethod` will invoke the referenced method on the said _object_ with compatible arguments. In the following [example](https://github.com/geraldnguyen/kitchensink/blob/main/java/src/test/java/core/MethodReferenceTest.java#L49), the `"hello "` string’s `concat` method is invoked with arguments `"a"`, `"b"`, `"c"` resulting in `"hello a"`, `"hello b"`, `"hello c"` respectively

```java
@Test  
void invoke_the_method_on_the_owning_object() {  
    var aList = List.of("a", "b", "c");  
    assertEquals(  
        List.of("hello a", "hello b", "hello c"),  
        aList.stream().map("hello "::concat).collect(Collectors.toList())  
    );  
}
```

The holder of the method reference is responsible to invoke it with compatible method arguments. In the following [example](https://github.com/geraldnguyen/kitchensink/blob/main/java/src/test/java/core/MethodReferenceTest.java#L58), we test `object::instanceMethod` with zero, one, two, or more parameters (using a custom functional interface). As long as the number and types of arguments are compatible with the instance method’s signature, the codes compile and run correctly.

```java
private String noParam(Supplier<String> supplier) {  
    return supplier.get();  
}  
private String oneParam(Function<String, String> function) {  
    return function.apply("1");  
}  
private String twoParam(BiFunction<Integer, Integer, String> function) {  
    return function.apply(1, 2);  
}  
private LocalDateTime threeParam(ThreeParam1 function) {  
    return function.apply(1, 2, 3);  
}  
  
interface ThreeParam1 {  
    LocalDateTime apply(int one, int two, int three);  
}  
  
@Test  
void invoke_with_compatible_arguments() {  
    // like a Supplier  
    assertEquals("a", noParam("a "::trim));  
    // like a Function or Consumer  
    assertEquals("a1", oneParam("a"::concat));  
    // like a BiFunction or BiConsumer  
    assertEquals("1", twoParam("0123"::substring));  
    // or any compatible Functional Interface  
    assertEquals(  
        LocalDateTime.of(2021, 10, 11, 1, 2, 3),  
        threeParam(LocalDate.of(2021, 10, 11)::atTime));  
}
```

# Class::staticMethod

The `Class::staticMethod` will invoke the referenced method on the declared _class_ with compatible arguments. In the following [example](https://github.com/geraldnguyen/kitchensink/blob/main/java/src/test/java/core/MethodReferenceTest.java#L135), `String` class’ static method `valueOf` is invoked with arguments `1`, `2.0`, `true` resulting in `"1"`, `"2.0"`, `"true"` string instances respectively

```java
@Test  
void invoke_the_static_method_on_the_class() {  
    var aList = List.of(1, 2.0, true);  
    assertEquals(  
        List.of("1", "2.0", "true"),  
        aList.stream().map(String::valueOf).collect(Collectors.toList())  
    );  
}
```

The holder of the method reference is responsible to invoke it with compatible method arguments. In the following [example](https://github.com/geraldnguyen/kitchensink/blob/main/java/src/test/java/core/MethodReferenceTest.java#L144), we test `Class::staticMethod` with zero, one, two, or more parameters (using a custom functional interface). As long as the number and types of arguments are compatible with the instance method’s signature, the codes compile and run correctly.


```java
private String noParam(Supplier<String> supplier) {  
    return supplier.get();  
}  
private String oneParam(Function<String, String> function) {  
    return function.apply("1");  
}  
private String twoParam(BiFunction<String, String, String> function) {  
    return function.apply("%s", "1");  
}  
private LocalDate threeParam(ThreeParam3 function) {  
    return function.apply(1, 2, 3);  
}  
  
interface ThreeParam3 {  
    LocalDate apply(int one, int two, int three);  
}  
  
@Test  
void invoke_with_compatible_arguments() {  
    // like a Supplier  
    assertEquals(System.lineSeparator(), noParam(System::lineSeparator));  
    // like a Function or Consumer  
    assertEquals("1", oneParam(String::valueOf));  
    // like a BiFunction or BiConsumer  
    assertEquals("1", twoParam(String::format));  
    // or any compatible Functional Interface  
    assertEquals(  
        LocalDate.of(1, 2, 3),  
        threeParam(LocalDate::of));  
}
```

# Class::instanceMethod

The `Class::instanceMethod` will invoke the referenced method on, of course, an instance of the said class. As I said earlier, this syntax is a bit special, because it isn’t clear what is that instance and who will provide it

Fortunately, the [below example](https://github.com/geraldnguyen/kitchensink/blob/main/java/src/test/java/core/MethodReferenceTest.java#L92) shed some light on that mystery. Let’s see how the `String::uppercase` method invocation gets its instance

```java
@Test  
void invoke_the_method_on_the_class_instance() {  
    var aList = List.of("a", "b", "c");  
    assertEquals(  
        aList.stream().map(a -> a.toUpperCase()).collect(Collectors.toList()),  
        aList.stream().map(String::toUpperCase).collect(Collectors.toList())  
    );  
}
```

It appears the JVM has a hand in this mystery. When `map` receives an object instance where `Class::instanceMethod` is specified, the JVM invokes that method (e.g. `toUpperCase()`) on the said object (e.g. `"a"`)

There is a catch, however. `toUpperCase()` takes in no parameter while this`map(...)` expects a `Function<String, String>` with one parameter. It turns out that the _functional interface_ has an extra parameter of the same type of declared class. As we established before, the JVM will then invoke the referenced method on that parameter.

In the following example, each `Class::instanceMethod` is invoked with an extra first argument of the same type as the declared class. The remaining arguments (if there are any) must be compatible with their signature

```java
private String noParam(Supplier<String> supplier) {  
    return supplier.get();  
}  
private String oneParam(Function<String, String> function) {  
    return function.apply("1   ");  
}  
private String twoParam(BiFunction<String, String, String> function) {  
    return function.apply("1", "2");  
}  
private LocalDateTime threeParam(ThreeParam2 function) {  
    return function.apply(LocalDate.of(2021, 10, 11), 2, 3);  
}  
  
interface ThreeParam2 {  
    LocalDateTime apply(LocalDate one, int two, int three);  
}  
  
@Test  
void invoke_with_compatible_arguments() {   // as long as the 1st param is a class's instance  
    // like a Supplier                                  // Constructor is neither static nor instance  
    assertEquals("", noParam(String::new));     // including here just to complete 0, 1, 2, 3 params  
    // like a Function or Consumer  
    assertEquals("1", oneParam(String::trim));  
    // like a BiFunction or BiConsumer  
    assertEquals("12", twoParam(String::concat));  
    // or any compatible Functional Interface  
    assertEquals(  
        LocalDateTime.of(2021, 10, 11, 2, 3),  
        threeParam(LocalDate::atTime)); // public LocalDateTime atTime(int hour, int minute)  
}
```

{{< pagebreak >}}

Checkout the code from [https://github.com/geraldnguyen/kitchensink/blob/main/java/src/test/java/core/MethodReferenceTest.java](https://github.com/geraldnguyen/kitchensink/blob/main/java/src/test/java/core/MethodReferenceTest.java)