+++
title = 'SOLID in Action: the Interface Segregation Principle'
subtitle = 'Many client-specific interfaces are better than one general-purpose interface'
date = 2023-01-27T08:00:00-07:00
tags = [ "Software Engineering", "SOLID principles", "Programming", "Object Oriented", "Interface Segregation"]
categories = ["Software Development"]
image = "0_2JS1H36QRQHFriO0.jpg.png"

+++

{{< figure src="0_2JS1H36QRQHFriO0.jpg" caption="Photos by [Eugen Str](https://unsplash.com/@eugen1980?utm_source=medium&utm_medium=referral) (left) and [Patrick](https://unsplash.com/@pf91_photography?utm_source=medium&utm_medium=referral) (right) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)">}}

{{< figure src="0_MRP8w8sESbbxCOzU.jpg" caption="Photos by [Eugen Str](https://unsplash.com/@eugen1980?utm_source=medium&utm_medium=referral) (left) and [Patrick](https://unsplash.com/@pf91_photography?utm_source=medium&utm_medium=referral) (right) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)">}}

If you are going to build a house, a table, or a chair, what is your preferred option? The multiple tools on the left or one Swiss knife on the right?

Yep, you are right. If you want to create something useful that last for years, you need specialized tools. The same thinking applies to software development, if you want to create a serious application, you need specialized components represented by their own, purposely designed interfaces. The Interface Segregation Principle (ISP) is available to guide the creation of those interfaces.

The popular description of this principle, by [Robert C. Martin](https://en.wikipedia.org/wiki/Robert_C._Martin), is

> Clients should not be forced to depend upon interfaces that they do not use

While researching the SOLID principle, I found an older, perhaps original, description of this principle in his 2000 paper [Design Principles and Design Patterns](https://web.archive.org/web/20150906155800/http://www.objectmentor.com/resources/articles/Principles_and_Patterns.pdf)

> Many client-specific interfaces are better than one general-purpose interface

Even though I find the original description is easier to understand, both are fine and should result in the same abstraction.

Flyable, Swimmable Animals
==========================

> Inspired by [this example](https://medium.matcha.fyi/solid-principles-using-typescript-b8d2cb84bfcf) from [Asa LeHolland](https://medium.com/u/ca267f562fa8?source=post_page-----6c8f92d2133a--------------------------------)

We have 3 separate interfaces representing Animals and abilities such as Flying and Swimming. They are _implemented_ by real animals such as Dog, Fish, and Bird. This is a fine illustration of how small, specific interfaces keep the code organized, maintainable, and extensible.

{{< figure src="1_ePi9aZt4Psn6WgMzSh-2vA.png" caption="Animal, Flyable, and Swimmable interfaces">}}

It looks good until we realized that it forgot to present the client’s perspective. In fact, there is no client at all!

Let’s assume that these interfaces and classes are part of a Pet Store application that shows a before-checkout questionnaire with relevant questions related to the selected items. If an animal is selected, a question about animal permits and noise regulation may be asked. If a flyable animal is selected, a concerning question about adequate flying space may be present. If a swimmable animal is selected, good access to clean water is in the questionnaire. So on and so forth.

Let’s also assume that we have many other animals than just Dog, Bird, and Fish in our collections.

{{< figure src="1_LywD8n6D2RsJvY4Lwwpj5g.png" caption="Animal, Flyable, and Swimmable interfaces: beyond">}}

Apply ISP to simplify client integration and improve the design and implementation quality

It is through this larger view showing both the component’s design and its client’s perspective that we understand the benefits of applying the ISP. Not only are the client-specific interfaces easier to integrate but it also simplifies the client’s implementation. Each questionnaire only concerns itself with a smaller set of attributes such as being Animal, or being Flyable… instead of dealing with multiple combinations of such attributes.

Backend for Frontend
====================

Popular in modern applications with multiple frontends, the [Backend-for-frontend (BFF) pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/backends-for-frontends) is pretty much a realization of the ISP principle.

{{< figure src="1_dqSwj5Y1Qy4E_LTeQZ6ZWw.png" caption="Separate backend for each frontend">}}


The premise is this: if your multiple frontends start requiring different supports from a single backend, it is time to create a separate backend for each frontend.

There are multiple considerations when implementing this pattern. The first presents the immediate concern: Cost. On-premise or on the cloud, it costs money to procure and expend computing, storage, IP address, domain name, development and maintenance time and manpower… The second, duplication of codes, even though is less important _now_ but will accumulate and roll over as technical debt and maintenance costs.

Conclusion
==========

The ISP is one that looks simple but requires extra thought. Your task is not limited to just the component at hand but the larger context of how the various client of the component is going to integrate with it. Your task also does not end at breaking down complex interfaces but also paying attention to the impact of doing so.


{{< include "posts/2022/12/becoming-a-solid-developer/series.include" >}}