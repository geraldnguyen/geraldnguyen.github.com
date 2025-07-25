---
title: "Web Application Development — #1: What makes a Web Application?"
subtitle: "Find out the basic characteristics of a web application and explore some common types of web applications"
date: 2023-02-13T12:00:00+07:00
categories: [Programming Tutorials]
tags: [Web Development, Programming, Web, Course, Features, JavaScript, Web Applications]
medium: https://javascript.plainenglish.io/web-application-development-1-what-makes-a-web-application-471a14739653
series: "Web Application Development Course"
image: 1_hCa3NClnycOrefkCGXUXLQ.png
---

*This is part of the [Web Application Development — An Introductory Course](../web-application-development-introductory-course/) series.*

A web application (or web app or webapp) has 2 distinct characteristics:

• It runs on the web  
• It leverages web technologies to deliver rich features to users

Let's explore each characteristic in detail.

## #1 A web application runs on the web

Web apps are everywhere nowadays. They can be the HR systems where employees log in to apply for leaves, the webmail portal for checking or sending emails, or the e-commerce websites, internet banking, search engines…

A user normally accesses a web application by entering that web app’s _readable address_ into a _browser_’s address bar. The browser resolves the address to locate the _web server_ which could be on the _internet_ or within a local _network_ or a larger enterprise network. The browser then downloads the _frontend codes_ of the web app from the located server and renders them on screens.

{{< figure src="https://miro.medium.com/v2/resize:fit:720/format:webp/1*oN73jGU4P27G3BouJWg-eg.png" caption="Webapp runs on the web" >}}

{{< figure src="https://miro.medium.com/v2/resize:fit:786/format:webp/1*kFPPZXVYTqVXepxOzvH37A.png" caption="Request sent by the browser" >}}

{{< figure src="https://miro.medium.com/v2/resize:fit:1100/format:webp/1*IyWjQl_yHDB_NNyp4NqqyQ.png" caption="Webapp's frontend code" >}}


In the example above `https://webmail.shaw.ca` is the readable address of Shaw's webmail web application. The web server is located at the internet IP address `64.59.175.71`. The browser downloaded the frontend codes and display them on the browser's window.

## #2 A web application leverages web technologies to deliver rich features to users

Running on the web, webapp provides its users with a rich set of features comparable to native or desktop applications.

{{< figure src="https://miro.medium.com/v2/resize:fit:640/format:webp/1*hCa3NClnycOrefkCGXUXLQ.png" caption="LinkedIn webapp looks very much like its native app counterpart" >}}

A webapp often features one or more of the following capabilities:

• Dynamic and interactive content and user interface  
• Task-oriented  
• Secured authentication and authorization  
• Able to access device capabilities  
• Able to work offline  
• Mashups

Let's look at some examples.

**The Mortgage Calculator** from [https://moneysmart.gov.au/home-loans/mortgage-calculator](https://moneysmart.gov.au/home-loans/mortgage-calculator) is a web application that is _task-oriented_ and features _dynamic and interactive content and a user interface_. To know how much future mortgage repayments can be, a user enters input such as loan amount and interest rate…. The calculator will then compute the repayment amount per selected frequency as well as a principle/interest breakdown chart.

{{< figure src="https://miro.medium.com/v2/resize:fit:1100/format:webp/1*quwdg8sxTeaeFOWV7jHGTQ.png" caption="Mortgage Calculator from moneysmart.gov.au" >}}

**Internet Banking** offers a clear example of _secure authentication and authorization_. There are 2 security features in the below screenshot. The gray lock icon on the top indicates that this webapp is protected by a valid SSL certificate which certifies the webapp identity and encrypts all its network communications between the browser and the server. The Login form in the middle collects the user’s credentials to _authenticate_ who the user claims to be. After the validation succeeds, the user will see only his/her accounts and not someone else’s. That’s authorization — ensuring the user can access only what he/she is authorized to.

{{< figure src="https://miro.medium.com/v2/resize:fit:640/format:webp/1*GTnXeavarDpADrTA2jkMnA.png" caption="Internet Banking security features - https://ib.techcombank.com.vn/servlet/BrowserServlet" >}}

Our next example, **Google Map**, demonstrates the webapp’s ability to access the location service of the user’s device (with the user’s permission, of course).

{{< figure src="https://miro.medium.com/v2/resize:fit:828/format:webp/1*bqnZ5MPiU_eqg84gBdK3pA.png" caption="Google Maps accessing device location" >}}

There are many other device capabilities to which a webapp can request access. For instance, if you have ever joined a Zoom/Team/Google Meet meeting on a browser, you have already used a webapp with Camera and Microphone capabilities.

{{< figure src="https://miro.medium.com/v2/resize:fit:1100/format:webp/1*_K7I6NRQoFX_kwvKadWZcw.png" caption="Google Maps accessing device location" >}}


**Working offline** is another feature found in Progressive Web App, a type of web application that blends native and mobile features. Normally, the webapp keeps a copy of itself in the browser’s local storage and runs that version when there is no internet. [https://tkctr.nvhung.nqtam.com/](https://tkctr.nvhung.nqtam.com/) is an e-book PWA that users can continue reading when offline.

{{< figure src="https://miro.medium.com/v2/resize:fit:1100/format:webp/1*KSaU7YJ1is1KwYv0eQcU8A.png" caption="Progressive Web App working offline" >}}

Last but definitely not least, mashups are very common nowadays. Mashups are the assembling and transforming of multiple data or information or assets from single or multiple sources into a more interesting or useful format. The example below from a Real Estate website 99.co illustrates that best. Different data about commute options, amenities, and attractions… are combined with Google Maps to show its users a comprehensive and intuitive map of a location.

{{< figure src="https://miro.medium.com/v2/resize:fit:1100/format:webp/1*tg1RuXEyZPDPRBK4qbd2kw.png" caption="Real estate website mashup with Google Maps" >}}

{{< pagebreak >}}

## Conclusion

We have examined the 2 basic characteristics of a web application, each with real examples.

In the next lesson, we will learn about 4 common types of web applications and how you may recognize them.

**Next:** [Web Application Development — #2: Four Types of Web Applications](../web-application-development-2-four-types-of-web-applications/)

{{< pagebreak >}}

For Content and Index, visit [Web Application Development — An Introductory course](../web-application-development-introductory-course/)

