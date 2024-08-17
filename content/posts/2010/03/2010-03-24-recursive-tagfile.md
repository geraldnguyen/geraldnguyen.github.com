---
title: "Recursive Tagfile"
subtitle: "This is my experience with recursive tagfile on Weblogic 10.3. I also describe a few problems encountered and how I solved them"
date: 2010-03-24T09:19:42+01:00
draft: false

categories: [Software Development]
tags: [j2ee, tagfile, debugging, old blog]
---

Tagfile is a good alternative for lazy people like me who hates doing java coding and XML configuration for simple task with lots of HTML codes.

But laziness knows no limit. Just now I ended up wasting haft a day on a recursive tagfile that would otherwise be completed within 1 hour at most. Yet it was an experience that's worth sharing.

The tagfile is named `catalogue.tag`. Its purpose is to display recursively a hierarchy tree of catalogues..

![](http://4.bp.blogspot.com/_g19EQrkxY30/S6ngDQvrxyI/AAAAAAAAABM/cNJh_45dWlI/s320/hier1.png)

The tagfile takes in an attribute named value of type `TreeNode`. This type contains properties call Object (for the actual object), Parent (of type TreeNode) and Children (List of TreeNodes). Below is how it can be invoked.

The recursion solution may appear very simple. So I thought.

My first attempt is shown below:

![](http://2.bp.blogspot.com/_g19EQrkxY30/S6nilSER_tI/AAAAAAAAABU/_omvFkdPJWc/s640/hier2.png)

Notice the red box. That's the cause of my error:

```
catalogue.tag:12:8: Current file's JSP version conflicts with current tag "huy:catalogue"'s.
```

As soon as I replace that with `tagdir` attribute, everything works. That seem strange but as long as it works I don't really care.

But the page seem to take long time to load. The CPU usage is high. I tried put in some time measurement to check how long the recursive takes; less than 30 ms every time. I'm really puzzled. It's definitely related to the recursive, but it seem more like how the Weblogic container prepares the recursive tagfile rather than how it invoke it.

