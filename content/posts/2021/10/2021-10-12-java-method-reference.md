---
date: 2021-10-12 09:19:42+01:00
draft: true
tags:
- Java
- KitchenSink
- lambda
- Method Reference
- Unit Testing
title: Java Method Reference
---
**Note 2024-08-15: An updated version of this article was published in Medium at https://medium.com/codex/java-method-reference-7b4f260bc3ad?source=user_profile---------85----------------------------**

-----------------

## Introduction

Method reference lets you use an existing method as a lambda as long as their signature (input & output) are compatible.

I first know method reference from C#, so its Java concept is familiar to me. Or that's what I thought until I picked up [Modern Java Recipes book](https://www.oreilly.com/library/view/modern-java-recipes/9781491973165/).

Here, we'll explore more about the 3 forms of method references in Java
- `object::instanceMethod`
- `Class::staticMethod`
- `Class::instanceMethod`

The last one is special, as we shall see

## object::instanceMethod

The `object::instanceMethod` will invoke the referenced method on the said object with compatible arguments. In the following example, the `"hello "` string object's `concat` method is invoked with argument `"a"`, `"b"`, `"c"` resulting in `"hello a"`, `"hello b"`, `"hello c"` respectively

```
@Test
void invoke_the_method_on_the_owning_object() {
    var aList = List.of("a", "b", "c");
    assertEquals(
        List.of("hello a", "hello b", "hello c"),
        aList.stream().map("hello "::concat).collect(Collectors.toList())
    );
}
```

The holder of method reference is entitled and responsible to invoke it with compatible method arguments

```
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

## Class::staticMethod

The `Class::staticMethod` will invoke the referenced method on the declared class with compatible arguments. In the following example, `String` class's static method `valueOf` is invoked with arguments `1`, `2.0`, `true` resulting in `"1"`, `"2.0"`, `"true"` respectively

```
@Test
void invoke_the_static_method_on_the_class() {
    var aList = List.of(1, 2.0, true);
    assertEquals(
        List.of("1", "2.0", "true"),
        aList.stream().map(String::valueOf).collect(Collectors.toList())
    );
}
```

The holder of method reference is entitled and responsible to invoke it with compatible method arguments

```
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
        threeParam(LocalDate::of)
    );
}
```

## Class::instanceMethod

The `Class::instanceMethod` will invoke the referenced method on, of course, an instance of the said class. The example below illustrates the `uppercase` method invocation on each string.

```
@Test
void invoke_the_method_on_the_class_instance() {
    var aList = List.of("a", "b", "c");
    assertEquals(
        aList.stream().map(a -> a.toUpperCase()).collect(Collectors.toList()),
        aList.stream().map(String::toUpperCase).collect(Collectors.toList())
    );
}

```

Question 1: Where does that instance come from? Answer: the holder of method reference will supply it.

Question 2: Is there a new syntax to bind separate object instance and method instance? Answer: Not really. The holder will call the method reference's functional interface method with one extra parameter - the Class's object instance. Then JVM internal does the wiring and invocation.

In the following example, each `Class::instanceMethod` is invoked with an extra first argument of type `Class`. The remaining arguments (if there are) must be compatible with its signature


```
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

@Test
void invoke_with_compatible_arguments() {       // as long as the 1st param is a class's instance
    // like a Supplier                          // Constructor is neither static nor instance
    assertEquals("", noParam(String::new));     // including here just to complete 0, 1, 2, 3 params
    // like a Function or Consumer
    assertEquals("1", oneParam(String::trim));
    // like a BiFunction or BiConsumer
    assertEquals("12", twoParam(String::concat));
    // or any compatible Functional Interface
    assertEquals(
        LocalDateTime.of(2021, 10, 11, 2, 3),
        threeParam(LocalDate::atTime));
}
```


Checkout the code from [https://github.com/geraldnguyen/kitchensink](https://github.com/geraldnguyen/kitchensink/blob/main/java/src/test/java/core/MethodReferenceTest.java)