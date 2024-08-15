---
title: "Code Review as an interview tool"
date: 2021-10-01T09:19:42+01:00
subtitle: Reviewing code as a way of learning and contributing
draft: true
weight: 50
tags: [java, kitchensink, interview, code review]
---

**Note 2024-08-15: An updated version of this article was published in Medium at https://medium.com/50ld/code-review-as-an-interview-tool-f57eb66e5da0?source=user_profile---------86----------------------------**

-----------------

In all software development teams that I have been part of, we always value and encourage every one to participate in code review. We consider it an important part in our Engineering Excellence guideline as a way of learning and contributing:

When you review other people codes, you help them and the team uncovered requirement/design/coding issues before they get into main codebase and eventually production. You also assure a consistent practice and coding standard are followed across all team members.

You also learn. If you're new or inexperienced, reviewing other people codes help you learn how certain tasks are acomplished. If you are experienced, code reviewing helps you keep track of how the codebase evolves. Furthermore, it will be a delightful surprise to see how new solutions pop up from time to time.

It is thus important that when we hire, we should evaluate candidates in their ability to review codes. 

For example, we could share a piece of codes and ask them to review. For example:

```
/**
 * This model class contains 2 properties `name` and `description`
 * It also contains method name `maxLength` which returns the longest
 * prefix whose reversed order is a suffix (e.g. `abXXXXba`) without overlapping
 *
 * Imagine a colleague create this class and submit it for code review,
 * please show us how you would review this code
 */
public class PrefixSuffixProblem {
    public String name;
    public String description;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public String maxLength(String str){
        String temp = "";
        for(int i=0;i<str.length()/2;i++   ){
        if (str.charAt(i) == str.charAt(str.length() - 1 - i)) {
        temp = temp + str.charAt(i);
    }
    else
        break;
        }
        return temp;
    }
}

```

Our goal is not to find someone with the same coding standard or preference, but someone serious about code quality and maintainability. We seek their opinions of what was done well, what not and how they would fix them (aka code suggestion)


Checkout the code from [https://github.com/geraldnguyen/kitchensink](https://github.com/geraldnguyen/kitchensink/blob/main/java/src/main/java/example/codereview/PrefixSuffixProblem.java)