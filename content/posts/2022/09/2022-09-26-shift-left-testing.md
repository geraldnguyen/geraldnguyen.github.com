---
title: "Shift Left Testing"
date: 2022-09-26T09:19:42+01:00
draft: true

tags: [Shift Left, Testing, Software Engineering]
---

**Note 2024-08-15: An updated version of this article was published in Medium at https://geraldnguyen.medium.com/shift-left-testing-b59ce9f9bf1e?source=user_profile---------57----------------------------**

-----------------


## What is Shift Left Testing?

A picture is worth a thousand words. Here are how Shift Left differs from the traditional approach

![](https://s7280.pcdn.co/wp-content/uploads/2017/07/key-1.png)

![](https://s7280.pcdn.co/wp-content/uploads/2017/07/key-2.png)

## How does my team _shift left_?

We started with **unit testing**, by developers, for their own codes. This is the easiest shift to implement, though it still tooks time, effort and patience to get it right. I recommend [Pragmatic Unit Testing in Java 8 with JUnit](https://pragprog.com/titles/utj2/pragmatic-unit-testing-in-java-8-with-junit/) if you want quickly level-up

The next step is to submit our codes to **static analysis testings** such as [Sonar](https://www.sonarqube.org/) and [Veracode](https://www.veracode.com/).

We plan to further shift left by adopting **Behavior-Driven Development** and involving our QA colleagues as early as the design stage. This is a challenging approach because it involves multiple groups of people with different skills and preferences. We'll need to find a concensus among them, appropriate attitudes and the right skillsets to get it working. I figure once we deliver a first success, the rest will follow more naturally.

## Reference

[Shift Left to Test Fast and Reliably](https://www.youtube.com/watch?v=iJkI8PwlxR4&t=1s)

My first introduction to Shift Left testing, documenting the real transition to Shift Left model during the migration of Microsoft Team Foundation Server (TFS) to Software-as-a-Service (SaaS) model

[Shift Left Testing: What, Why & How To Shift Left](https://www.bmc.com/blogs/what-is-shift-left-shift-left-testing-explained)

A good overview of what is Shift Left and how it differs from traditional approach


