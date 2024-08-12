---
title: "Consistency in framework design"
date: 2011-01-06T09:19:42+01:00
draft: false
weight: 50
tags: [architecture, design, old blog]
---

> Framework: a set of ideas, principles, agreements, or rules that provides the basis or outline for something intended to be more fully developed at a later stage (MSN Dictionary)

Not sure if you agree, but for me, the above definition implies "consistency" as a essential effect of using a framework. In the context of software development, using a framework usually equals to following its design guideline, or  implementing/extending its interfaces/base classes. Things can stuck or go wrong and time will definitely be wasted if you don't.

I have only used 4 different application framework (.NET framework not included since it is comparably as big as Java standard API). 2 of them, Struts 2 and Spring MVC do not force me to explicitly implement their own interfaces, but implicitly I still have to. Such is due to expectation such as "execute" method returning a String in Struts 2 Action class or a controller method returning a ModelAndView in Spring MVC Controller classes. Futhermore, the fact that these semantically identical classes carrying different names in different frameworks (Action, Controller) clearly proves the influence of framework's design on the implementation details. The other 2 frameworks, both in-house, explicitly requires extending framework's base classes.

So, for me, consistency is expected if I use a framework. It is even more desirable if I build one.

But it didn't come as plentiful as I have hoped. 

My current project is to build an in-house application framework. I am the team lead. There is no explicit project manager, so I assume I am (but did it poorly). At the beginning, there are little to develop and review, so the structure, design and implementation are pretty consistent. But when I started delegating design and implementing tasks to my team members, the level of consistency just kept getting lower. It's still manageable, I think. But I wish if I could keep it consistent throughout, I would have saved myself lots of time trying to persuade other members to refactor their design. 

So, the lessons for me are:
- Communicate the goal, design and implementation standard to all team members. Make sure they agree and abide by them. In my situation, I didn't even have a design and implementation standard. So...
- Establish a design and implementation standard that all team members accept.
- Review the design and implementation  and compare them with the agreed design and implementation standard. Make this a regular and seemingly serious activity. In my situation, I didn't review all of them; and didn't do it regularly. And I didn't make it seem serious enough.
- Do not be afraid to discuss the design and implementation with your member. And willing to defend your idea (but be open to their reasonable arguments). In my situation, I sometimes afraid to discuss because my arguments are weak when there is no standard to back me up (and I'm afraid of being seen micro-managing) .
- Set example for the rest to follow. It's a human nature. 

Among the above lessons, the most urgent and also very important one is to establish a standard. That's what I will do next. Better late than never.