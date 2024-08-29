---
title: What Are APIs?
subtitle: A non-technical explanation of APIs and where they belong
date: 2022-12-27T09:19:42+01:00
image: 0_KnGqks4dktk7pUv3.jpg
draft: false
categories: [Software Development]
tags: [API, Frontend, Backend, Analogy]
---

# Are APIs the waiters?

{{< figure src="0_u9-gya6X9E94NV5o.webp" caption="Source: [Reddit](https://www.reddit.com/r/ProgrammerHumor/comments/qq2m9g/explained_frontend_backend_apis/) and LinkedIn">}}


# No — The waiters are part of Frontend.

They are responsible for creating a nice visual and interactive experience such as greeting customers, arranging the tables, present the dishes — like what React or Angular codes do.

They will take your orders — like how HTML forms accept your inputs, auto-complete your sentences, or apologize that certain dishes aren’t available.

And they send your orders to the kitchen. Like what [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) or [Axios library](https://axios-http.com/) does to invoke _APIs_ in the Backend.

# APIs belong in the Backend

With the Kitchen being Backend, APIs are the kitchen’s offering of available dishes to waiters and customers. For example, the API `GET /kitchen/dishes` tells its consumers what they can order today, the `GET /kitchen/dishes/hawaian-pizza` returns a detailed description of that delicious food, or `POST /kitchen/tables/123/orders` accepts a new order.

Fulfillment of API requests is of course completed in the Kitchen. The chef determines the menu, set out the recipe for each dish, and cooks all customer orders.

# There may not be a Frontend

{{< figure src="0_KnGqks4dktk7pUv3.webp" caption="Photo by [Anthony Fomin](https://unsplash.com/@aginsbrook?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)" >}}


It’ll be you calling the API directly. Fret not, it is simple.

If you are one of the queuing customers in the above photo, I’m sure you know how to order. Just browse the menu, look at the pictures (and their prices), then says the name of your favorite ice cream or point to it.

It’s mostly the same with APIs. You call `GET /ice-creams/` to learn what is available, then `GET /ice-creams/chocolate-special` to check out its picture, price, sizes…, and finally `POST /orders/` to request your well-deserved treats.

# What are APIs?

API stands for Application Programming Interface. It refers to how software components should _publicly_ appear to each other. When one component needs another to do some specific work, it calls a specific service defined in the other component’s API. In our restaurant example above, when a Waiter needs the Kitchen to cook an Order, the Waiter calls the `cook(Order)` service defined in the Kitchen’s API.

API originally applies to software codes or libraries that are packaged with the invoker in a single `.exe` file or deployed to a program folder. It still does.

But API nowadays also applies to software separated over a network or even the internet. When we open any of the popular websites such as Facebook, Youtube, The Guardian, etc…, chances are there are dozen, even hundreds of API calls made to servers hundreds of miles or even halfway across the globe every minute. At such distance, these software are no longer called libraries or modules like their former cousin but take the name _services_ or _APIs_ themselves.

API does more than perform a service. It can also return data. Facebook APIs do not just log you in, it returns dozen of posts from your friends and relatives to fill up your feed. Youtube APIs don’t just help you like a video, it returns the frames that make up the video itself.

In conclusion:

{{< quote  `APIs are **the data or services that some software codes, modules, libraries, or websites have to offer to outsiders**` />}}

If we are to go beyond the realm of software, _APIs are the goods or services that someone or something has to offer to outsiders_. APIs are thus everywhere. Does this competitor to Amazon have a chance?

{{< figure src="0_ONAGo4F25oRjxC8e.webp" caption="Photo by [HamZa NOUASRIA](https://unsplash.com/@hamza01nsr?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)" >}}

