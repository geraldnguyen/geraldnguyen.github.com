---
title: "Java Stream Interview Tests - takeWhile"
date: 2021-11-01T09:19:42+01:00
lead: Deeper understanding of Stream's mechanics
draft: false
weight: 50
tags: java, stream, interview, coding test
---

## Requirement

Re-implement `takeWhile(Predicate)` method

## Discussion

`takeWhile(Predicate)` (and `dropWhile(...)`) is a *stateful* intermediate method. The maintenance of state is necessary to implement such operation

`takeWhile` is essentially a filtering mechanism where elements are accepted so long as the predicate is still `true`. Once the predicate resulting in `false`, all subsequence elements are dropped from the stream. A stateful variable is necessary to record the change of state trigger by that event.

```
boolean stop = false;
List<Integer> result = new ArrayList<>();

for (Integer i : ints) {
    if (i > 0 && !stop) {
        result.add(i);
    } else {
        stop = true;
    }
}

return result;
```

## Stream-based Solution

An Stream-based solution can be derived based on above loop-based codes. 

Due to stream's requirement of referenced local variable being `final` or effective final, the `boolean stop = false` must be replaced by an object whose mutable state represents the binary boolean values. A `Map<String, Boolean> stopFlag = new HashMap<>();` ought to do it.

To modify the state variable, `forEach` provides an obvious alternative to `for (Integer i : ints)`. 

OR we can use `peek` to modify the state followed by a `filter` to enforce the state-dependent behavior

```
public static List<Integer> takeWhile4(List<Integer> ints) {
    Map<String, Boolean> stopFlag = new HashMap<>();

    return ints.stream()
        .peek(i -> {
            if (i <= 0) {
                stopFlag.put("stop", true);
            }
        })
        .filter(i -> !stopFlag.containsKey("stop"))
        .collect(Collectors.toList())
        ;
}
```

One downside to above solution is that `peek` is intended for debugging purpose and may be considered a code smell.

We can do away with `peek` by having another method mutating the state e.g. `map`. Since `map(...)` has to return something, we can try returning `null` when state changes and having a `.filter(Objects::nonNull)` afterward to drop all `null` ineligible elements

```
public static List<Integer> takeWhile3(List<Integer> ints) {
    Map<String, Boolean> stopFlag = new HashMap<>();

    return ints.stream()
        .map(i -> {
            if (i > 0 && !stopFlag.containsKey("stop")) {
                return i;
            } else {
                stopFlag.put("stop", true);
                return null;
            }
        })
        .filter(Objects::nonNull)
        .collect(Collectors.toList())
        ;
}
```

## Alternative

An non-boolean-state alternative involves identifying the exact point where state changes **prior** to extracing the elements. 

A fixed list can afford us such random look-ahead

```
public static List<Integer> takeWhile5(List<Integer> ints) {
    int firstNonPositiveIdx = -1;
    for (int i = 0; i < ints.size(); i++) {
        if (ints.get(i) <= 0) {
            firstNonPositiveIdx = i;
            break;
        }
    }

    if (firstNonPositiveIdx < 0) {
        return ints;
    }
    return ints.subList(0, firstNonPositiveIdx);
}

```

or

```
public static List<Integer> takeWhile4(List<Integer> ints) {
    int firstNonPositiveIdx = IntStream.range(0, ints.size())
        .filter(idx -> ints.get(idx) <= 0)
        .findFirst().orElse(-1);
    if (firstNonPositiveIdx < 0) {
        return ints;
    }

    return ints.stream().limit(firstNonPositiveIdx).collect(Collectors.toList());
    // alternatively we can `IntStream.range(0, firstNonPositiveIdx)` to just pick the element out of ints
}
```


## Sample Codes

Checkout sample [solutions](https://github.com/geraldnguyen/kitchensink/blob/main/java/src/main/java/example/codingtest/TakeWhilePositive.java) and [tests](https://github.com/geraldnguyen/kitchensink/blob/main/java/src/test/java/example/codingtest/TakeWhilePositiveTest.java) in my [github](https://github.com/geraldnguyen/kitchensink)
