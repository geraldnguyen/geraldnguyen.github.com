+++
title = 'SOLID in Action — the Single Responsibility Principle'
subtitle = '“A class should have one, and only one, reason to change.”'
date = 2022-12-14T08:00:00-07:00
tags = [ "Software Engineering", "SOLID principles", "Software Development", "Object Oriented"]
categories = ["Software Development"]
image = "0_AzgjytmEOFKBuWDZ.jpg"
medium = 'https://medium.com/codex/solid-in-action-the-single-responsibility-principle-7cb70c32cc03'
+++

{{< figure src="0_AzgjytmEOFKBuWDZ.jpg" caption="Photo by [Robert Linder](https://unsplash.com/@rwlinder?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)" >}}

We know the saying “Jack of all trades, master of none”. We dislike it when we are at the receiving end. We dislike it too when it is said to someone we love. That should extend to _things_ that we love too.

We developers love the codes we created. We are proud of the function we write, the class we design, the component, the system, and the microservices… we develop. We thus would not like it if they become “Jack of all trades, master of none”.

Hence we should always keep in mind the Single Responsibility principle (SRP) when we work on new creations.

There are 2 versions of this principle:

*   Each class should have a single responsibility
*   A class should have one, and only one, reason to change

The latter is probably more famous, counting [22,600 results](https://www.google.com/search?q=%22A+class+should+have+one%2C+and+only+one%2C+reason+to+change%22&sxsrf=ALiCzsa2EBcvHhvDFVddG8qcLDNE_9YrBA%3A1670480497947&ei=cYKRY5uVOb6dseMP7PKsuAk&ved=0ahUKEwjbkbzlsOn7AhW-TmwGHWw5C5cQ4dUDCA8&uact=5&oq=%22A+class+should+have+one%2C+and+only+one%2C+reason+to+change%22&gs_lcp=Cgxnd3Mtd2l6LXNlcnAQAzIECCMQJzIECAAQHjIECAAQHjIFCAAQhgMyBQgAEIYDMgUIABCGAzIFCAAQhgM6BwgjELADECc6CggAEEcQ1gQQsANKBAhBGABKBAhGGABQ-xZY5yNg6CVoAXABeACAAYgBiAH9AZIBAzAuMpgBAKABAcgBCcABAQ&sclient=gws-wiz-serp) on google. But I feel the former is easier to understand and relates better to whatever names we give to our creations. Regardless, the guidance is the same.

Reusable, Maintainable, and Understandable
==========================================

In the [first article]({{< ref "becoming-a-solid-developer" >}}), I suggest that an `EmailService` should concern itself with sending emails only, not sending emails _and_ updating order status.

{{< figure src="0_b2-Zh2F-uSHYQcOH.png" caption="EmailService and its associations" >}}

The benefits are multiple:

*   `EmailService` is more **reusable**. `StockManagementService` can also call it to raise “low stock” email alerts.
*   `EmailService` is more **maintainable**. We need to take care of one `sendEmail(EmailContent)` method and nothing else. We do not need to know the order management logic nor worry about how it may change to add a new status.
*   `EmailService` is more **understandable**. Without looking at a single line of code, we know with confidence that it is responsible for only sending emails.

Beware: Feature Evolvement
==========================

As the application grows, the users demand more. Emails need to be better formatted e.g. in HTML, with design images and CSS. The development team decided to capture the complex email designs in template files instead of hardcoding them in codes.

{{< figure src="1_O_FdHcQ6a7JjYmlmRtomjw.png" caption="Order confirmation (screenshot)" >}}


`EmailContent` is now extended to include 2 new fields: a `templateName` holding the name of the template file and `templateValues` holding the values of placeholders defined in the template (e.g. `customerName` maps to `Huy`, `orderAmount` maps to `42.90`, `orderCurrency` maps to `SGD`)

```
data class EmailContent (  
   // existing fields  
   val fromEmail: String,   
   val toEmail: String,   
   val ccEmail: String,  
   val emailBoy: String,  
   // new fields  
   val templateName: String,     
   val templateValues: Map<String, Object>   
)
```

The team also added one new method to `EmailService` to read the `templateName`, fill it with values from `templateValues` , and send out the final email.

```
interface EmailService {  
    fun sendEmail(content: EmailContent): String  
    fun sendEmailWithTemplate(content: EmailContent): String      
}
```

Unfortunately, this is a violation of the SRP. The new method adds template processing responsibility to the `EmailService`. While `EmailService` can still be reusable, it is obviously less maintainable and less understandable due to the extra logic.

Though this violation is still easy to spot, it is a common pitfall. However good the original design is, feature evolvement is often involved in the violation of the SRP.

The lesser evil
===============

The business blooms and added 2 more modes of ordering. In addition to the original shopping cart ordering, orders can come from affiliates or monthly recurrent subscriptions. Regardless of which mode, customers expect confirmation emails when an order is created and their credit card is charged.

To avoid repeating the codes that create and send confirmation emails in 3 different classes, the team decided to move them to `EmailService`’s `sendOrderConfirmation(OrderDetail)` method

{{< figure src="1_J1jUzedz3xlzOmlj2YdIiQ.png" caption="Avoid code duplication or be true to the SRP" >}}

This is an obvious violation of the SRP, but it has strong justification. Which is the lesser evil, code duplication in 3 classes, or having 1 more responsibility in `EmailService`?

Anthological Responsibility
===========================

Have you watched The Ballad of Buster Scruggs, an [anthology film](https://en.wikipedia.org/wiki/Anthology_film) by the Coen brothers? It is a collection of 6 short western stories.

By analogy, let’s have an anthology class. Perhaps its best examples are utility classes.

A common theme usually connects the individual reasons within the larger group. [StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html), for instance, collects many string manipulation methods.

What if there is no clear theme? A `MiscellaneousUtils` (or its shorter form `MiscUtils`) is fine.

Conclusion
==========

The Single Responsibility principle brings many benefits. Reusability, maintainability, and understandability are just some of them.

SRP is not always achievable or maintainable, unfortunately. We analyzed two scenarios where despite a good initial design, violation still manages to creep in.

We have not discussed how to resolve those violations. That is on purpose because fixing them is as hard as fixing the technical debts every team accumulates over time.

Last but not least, we discuss utility class as a form of anthological responsibility.

{{< include "posts/2022/12/becoming-a-solid-developer/series.include" >}}