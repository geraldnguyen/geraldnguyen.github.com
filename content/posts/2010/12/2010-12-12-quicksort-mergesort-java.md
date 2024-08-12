---
title: "Quicksort vs Merge sort in Java"
date: 2010-12-12T09:19:42+01:00
draft: false
weight: 50
tags: [java, algorithm, sort, old blog]
---

The title is a little larger that what I have in this entry :D

If you want the answer fast, go here http://www.coderanch.com/t/520171/java/java/Why-Collections-sort-merge-sort#2355668

And here is a longer write-up:

I participated in an thread in Javaranche regarding why Arrays uses quicksort and Collections use merge sort (http://www.coderanch.com/t/520171/java/java/Why-Collections-sort-merge-sort). The thread starter claimed that quicksort is the best sort algorithm, for which I disagreed. I also replied with a link to wikipedia about merge sort performs better when the underlying datatype does not support random access (as does array). I thought that was the reason why Collections use merge sort: because a collection type such as LinkedList does not necessarily support random access.

I had some doubt then, though. I did noticed "This implementation dumps the specified list into an array, sorts the array" in the javadoc and wondered why wasn't quicksort be used. But lazy me doesn't want to do further googling and reading; I replied with the right-but-was-not-correct arguments.

Later, I checked that thread again and found an answer from a user named "Martin Vajsar". His explanation is brilliant, but let me rephrase them here:
- Merge sort is stable. Quicksort may not, especially if the implementation must be efficient.
- Stable is important for sorting object. That's why merge sort is used in Collections and `Arrays.sort (Object[] ...)`. (Yes, it's true. A`rrays.sort (Object[] ...)` uses merge sort!)
- Stable is not important for primitive types, since 2 primitive values are indistinguishable if they are equals. And quicksort may be better than merge sort when operating on array.

There is another bonus information from Martin: "Mergesort's ability to efficiently sort sequentially accessed data is of no real use in Java. This feature is used when sorting data so large that they don't fit to memory and must be written to disk"