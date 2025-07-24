---
categories:
- Software Development
- Career Development
date: 2021-10-30 09:19:42+01:00
draft: false
image: 1_BhOX_9_jetMTNa5WYqHqsQ.jpg
medium: https://medium.com/50ld/java-stream-interview-tests-distinct-characters-741cc82cbdd6
sutitle: Stream has been an important part of Java development since Java 8. This
  test assesses candidates familiarity with various stream operations
tags:
- Java
- stream
- interview
- coding test
title: Java Stream Interview Tests - Distinct Characters
---

Requirement
===========

Given a sentence comprising of one or multiple words separated by space (e.g. `java`, `Hello World`, `aadsbbaba`), return a sorted list of distinct characters excluding space

Solve this problem using Java Stream

**Hint**: Use `String.split(“”)`to split a string into a `String[]`, then use `String.charAt(0)` to convert each single-char string to `char`

Discussion
==========

This problem tests candidate’s knowledge of the following `Stream` methods:

*   `Stream.of(...)` or `Array.stream(...)` to convert an array of single-char string (e.g. from `str.split("")`) to `Stream<String>`
*   `map(str -> str.charAt(0))` to convert `String` to `Character`. **Note**: while `String.toCharArray` can readily convert a `String` to an `char[]`, turning the resulting array to a `Character` stream isn't straightforward due to lack of built-in support for `char` type in Stream and Collection API. The `Arrays.asList(str.toCharArray()` or `List.of(...)` results in a `List<char[]>` instead
*   `distinct()` to keep only 1 instance of each encountered character
*   `sorted()` to sort the encountered characters in their natural order
*   `collect(Collectors.toList())` to collect stream elements into a list

Sample Solution
===============

A sample solution is shown below. In addition to above operations, we only utilize an extra `filter(...)` operation to discard spaces from getting into our response

```java
public static List<Character> distinctCharsStream2(String sentence) {  
    return Arrays.stream(sentence.split(""))  
        .map(s -> s.charAt(0))  
        .filter(i -> i != ' ')  
        .distinct()  
        .sorted()  
        .collect(Collectors.toList());  
}
```

Recall again the use of `String.split("")` and `String.charAt(0)` instead of `String.toCharArray()`. Due to the lack of built-in support for `char` type in Stream and Collection API, `Arrays.asList(sentence.toCharArray())` or `List.of(sentence.toCharArray())` result in a `List<char[]>` instead of `List<Character>` that we need

Extra
=====

Instead of a sentence string, candidate shall be given an list of words.

This modification results in mostly the same solution as the original problem, with 2 twists:

*   Spaces are omitted
*   Candidate may use `flatMap` to merge characters from each word into a single `Stream<Character>`

```java
words.stream()  
    .flatMap(word -> Stream.of(word.split("")))  
    .map(s -> s.charAt(0))  
    ...
```

A small modification to the sample solution can lead us to the same `flatMap(...)` usage

```java
return Arrays.stream(sentence.split(" "))  
        .flatMap(word -> Stream.of(word.split("")))  
        ...
```

Imperative Solution
===================

If we omit the Stream requirement, some candidate may derive an imperative solution using loop and `Set`

```java
Set<Character> characters = new HashSet<>();  
for (char c : sentence.toCharArray()) {  
    if (c == ' ') continue;  
    characters.add(c);  
}  
List<Character> result = new ArrayList<>(characters);  
result.sort((c1, c2) -> c1 - c2);  
return result;
```

There is nothing wrong with this approach. To some extend, I think it is even more understandable than the Stream-based one.

But as functional and reactive programming become more and more popular, so does the need to master Stream operations. If there are still time, I would politely ask the candidate to convert his/her codes to Stream.

{{< figure src="1_BhOX_9_jetMTNa5WYqHqsQ.jpg" caption="distinct characters" >}}

Sample Codes
============

Checkout sample [solutions](https://github.com/geraldnguyen/kitchensink/blob/main/java/src/main/java/example/codingtest/DistinctCharacters.java) and [tests](https://github.com/geraldnguyen/kitchensink/blob/main/java/src/test/java/example/codingtest/DistinctCharactersTest.java) in my [github](https://github.com/geraldnguyen/kitchensink)
