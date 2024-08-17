---
title: "Code Review as an interview tool"
subtitle: Reviewing code as a way of learning and contributing
image: 1_Fcg_f8TQsK2xwbmK9xhAQA.jpg
date: 2022-11-22T09:19:42+01:00
tags: [Software Engineering, Code Review, Interview]
categories: [Software Development, Career Development]
---

## Rationale

In all software development teams that I have been part of, we always value and encourage everyone to participate in code reviews. We see it as an effective way of learning and contributing. It has an important place in my Engineering practice.

We learn when we review other people’s codes. That’s actually an effective way of keeping up-to-date with what is happening to the code base, what sort of problems our colleagues are trying to solve, and what techniques they employed to solve them. Whether you are young or experienced, the delightful feeling of seeing creative solutions during code reviews is the same.

We also helped our teammates when we review their codes uncovering requirement/design/coding issues before they get into the main codebase and eventually production. We also help ensure consistent practice and coding standards are followed by all team members.

{{< figure src="1_Fcg_f8TQsK2xwbmK9xhAQA.jpg" caption="Interviewing">}}

## As an interview tool

Thus, when we hire, we should evaluate candidates in their ability to review codes and how much they care about code quality and maintainability.

Often, we share a piece of code (e.g. below) and ask them to review it. We seek their opinions of what was good in the code, what was not, and how they would fix them.

```java
/**  
 * This model class contains 2 properties \`name\` and \`description\`  
 * It also contains method name \`maxLength\` which returns the longest  
 * prefix whose reversed order is a suffix (e.g. \`abXXXXba\`) without overlapping  
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
        String temp \= "";  
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

In the example above, if your candidate noticed several formatting problems in method `public String maxLength(String str) {...}`, their codes would likely be legible and nicely and consistently indented. Your eyes would thank you for giving a plus (+) to him or her.

If your candidate pointed out how `public` modifier is misused in `public String name` declaration, he/she knows about the Java bean contract. When asked how it can be improved or simplified further if the response is to delete all the getters, setters, congratulation — you may have someone knowing more than just the basic bean contract.