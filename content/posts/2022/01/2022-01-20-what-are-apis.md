---
title: "What are APIs?"
date: 2022-01-19T09:19:42+01:00
draft: true

tags: [API]
---

**Note 2024-08-15: An updated version of this article was published in Medium at https://geraldnguyen.medium.com/what-are-apis-2259bb43ddb4?source=user_profile---------73----------------------------**

-----------------

## Are API the waiters?

![](/images/backend-frontend-api-medium.jpg)

## No - The waiters are part of Frontend. 

They are responsible for creating the nice visual and interactive experience such as greeting customers, arranging the tables, present the dishes - like what React or Angular codes do.

They will take your orders - like how HTML forms accept your inputs, auto-complete your sentences, or apologize that certain dishes aren't available.

And they send your orders to the kitchen. Like what [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) or [Axios library](https://axios-http.com/) does to invoke *APIs* in the Backend.

## APIs belongs in the Backend

With the Kitchen being Backend, APIs are the kitchen's offering of available dishes to waiters and customers. For examples, the API `GET /kitchen/dishes` tells its consumers what they can order today, the `GET /kitchen/dishes/hawaian-pizza` returns detailed description of that delicious food, or `POST /kitchen/tables/123/orders` accepts a new order.

Fulfillment of API requests are of course completed in the Kitchen. The chef determines the menu, set out recipe of each dish, and cooks all customer orders.

## There may not be a Frontend

![](/images/icecream-truck-small.jpg)

It'll be you calling the API directly. Fret not, it is simple.

If you are one of the queuing customers in above photo, I'm sure you know how to order. Just browse the menu, look at the pictures (and their prices), then says the name of your favourite ice cream or point to it.

It's mostly the same with APIs. You call `GET /ice-creams/` to learn of what are available, then `GET /ice-creams/chocolate-special` to check out its picture, price, sizes..., and finally `POST /orders/` to request your well-deserved treat.

## What are APIs?

What are APIs? There are plenty of [technical definitions](https://www.google.com/search?q=what+are+apis&oq=what+are+apis&aqs=chrome..69i57j0i512l6j69i60.3936j0j4&sourceid=chrome&ie=UTF-8) out there, but here is my own's lesser definition: **It's way for a system to offer its goods or services to outsiders**.

It isn't just kitchen and foods. APIs are everywhere nowadays. Do you like books?

![](/images/old-book-seller.jpg)
