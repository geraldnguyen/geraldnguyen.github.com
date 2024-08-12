---
title: "Java Stream Interview Tests - Distinct Characters"
date: 2021-10-30T09:19:42+01:00
sutitle: Find distinct characters in a sentence (excluding space)
draft: false
weight: 50
tags: [java, stream, interview, coding test]
---

## Requirement

Given a sentence comprising of one or multiple words separated by space, return a sorted list of distinct characters excluding space

Solve this problem using Java Stream

**Hint**: Use `String.charAt(0)` to convert a single-char string to `char`

## Discussion

This problem tests candidate's knowledge of the following `Stream` methods:
- `Stream.of(...)` or `Array.stream(...)` to convert an array of single-char string (e.g. from `str.split("")`) to `Stream<String>`
- `map(str -> str.charAt(0))` (Note: while `String.toCharArray` can readily convert a string to an char array, turning the resulting array to a `Character` stream isn't straightforward due to lack of built-in support for `char` type in Stream and Collection API. The usual `Arrays.asList(str.toCharArray()` or `List.of(...)` results in a `List<char[]>` instead)
- `filter(c -> c != ' ')`
- `distinct()`
- `sorted()`
- `collect(Collectors.toList())`

If given a list of words instead of a sentence string, candidate may opt to use `flatMap` to merge characters from each word into a single `Stream<Character>`

```java
words.stream()
    .flatMap(word -> Stream.of(word.split("")))
    .map(s -> s.charAt(0))
    ...
```

## Alternatives

If we omit the Stream requirement, some candidate may derive a pre-Java-8 approach using loop and `Set`

```
Set<Character> characters = new HashSet<>();
for (char c : sentence.toCharArray()) {
    if (c == ' ') continue;
    characters.add(c);
}
List<Character> result = new ArrayList<>(characters);
result.sort((c1, c2) -> c1 - c2);
return result;
```

If that's the case, we can ask them to convert their solution to a Stream-based solutions to test their familiarity and experience with Stream-based approach

## Sample Codes

Checkout sample [solutions](https://github.com/geraldnguyen/kitchensink/blob/main/java/src/main/java/example/codingtest/DistinctCharacters.java) and [tests](https://github.com/geraldnguyen/kitchensink/blob/main/java/src/test/java/example/codingtest/DistinctCharactersTest.java) in my [github](https://github.com/geraldnguyen/kitchensink)
