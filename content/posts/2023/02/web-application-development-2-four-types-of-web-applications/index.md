---
title: "Web Application Development — #2: Four Types of Web Applications"
subtitle: "And how you may recognize them"
date: 2023-02-23T12:00:00+07:00
categories: [Programming Tutorials]
tags: [Programming, Web Development, Single Page Applications, Progressive Web App, Coding, JavaScript, Course]
medium: https://javascript.plainenglish.io/web-application-development-2-four-types-of-web-applications-61036240796
series: "Web Application Development Course"
image: 0_IooeYABogmJ5V4s1.jpg
---

*This is part of the [Web Application Development — An Introductory Course](../web-application-development-introductory-course/) series.*

We can classify web applications into 4 types:

• Client-side application  
• Server-side application  
• Single Page application  
• Progressive web app

Let's explore each type in detail.

## Client-side Application

Some web applications operate only on the client side i.e. the browser.

The earliest client-side web applications are probably Applets which were small applications capable of executing within browsers. Applets became popular since Java 1.0's introduction in 1995 but gradually lost browser support amid competition from other technologies and security concerns.

{{< figure src="0_IooeYABogmJ5V4s1.jpg" caption="Java Chess Applet example - https://en.wikipedia.org/wiki/Java_applet#/media/File:ChessApplet.png" >}}

Between 2000 and the first half of the 2010s, Flash was a popular option to develop web applications which comprises mostly games and multimedia applications. Flash retreated in the second half of the 2010s due to security concerns and the advancement of web technologies.

{{< figure src="https://miro.medium.com/v2/resize:fit:786/format:webp/0*YoJfY-QnvRr0JobZ.jpg" caption="Flash game example from Game Informer - Source: https://www.gameinformer.com/b/features/archive/2010/11/28/gem-polishers.aspx" >}}

Nowadays, Javascript, HTML5, and CSS3 dominate the landscape of web application technologies. Single-player games and simple utilities (e.g. calculators, converters) probably dominate this type of client-side web application.

{{< figure src="https://miro.medium.com/v2/resize:fit:1100/format:webp/1*eBmPIQ34p0yZQwbrj2uGWQ.png" caption="Modern JavaScript Pong game example - Source: https://www.ponggame.org/" >}}

## Server-side Application

This type of web application runs _mostly_ on the server. Server-side applications still have user interface components aka web pages that render in browsers. These pages often contain Javascript and CSS as well. But these features are usually reserved for display and simple input validations. The bulk of input processing, data storage and retrieval, and process execution happens on the server side. Each time a user requests new data, triggers an execution, or submits inputs, the browser reloads the entire web page. The server receives and processes the request, generates the HTML output, and sends it back to the browser. The whole page, input, HTML, CSS, and images travel across the internet for _every_ request.

In the beginning, server-side applications were developed by hardcore programmers writing C, C++, or Perl before it became a bit easier with the advent of `.php`, `.asp`, `.jsp` and `/servlet/` server page technologies.

{{< figure src="https://miro.medium.com/v2/resize:fit:1100/format:webp/0*GZAVjoTzJ6neDlyG.png" caption="Perl CGI form example - Source: https://linuxconfig.org/perl-cgi-form-submit-example" >}}

Nowadays, most server-side applications do not advertise their dot-extension technology anymore. URL mapping techniques allow server programs to receive browser requests at `.html` or no extension addresses.

{{< figure src="https://miro.medium.com/v2/resize:fit:640/format:webp/1*6skyt5qAD0CzRoEIY-wrjg.png" caption="Popular server-side application frameworks" >}}

## Single Page Application

Single Page Application (SPA) is the newest fashion. It combines the best of client-side rendering and interaction and server-side security and processing.

A SPA appears as a single web page to end users. Most, if not all, interactions with the app happen within the page regardless of whether they are form submission, file upload, data retrieval, screen refresh, or even page navigation.

{{< figure src="https://miro.medium.com/v2/resize:fit:1100/format:webp/1*A43NRUzitgp5okwcQJk36w.png" caption="Facebook as an example of Single Page Application" >}}

Powering SPA are modern application frameworks on both client and server sides. On the frontend, Angular, ReactJS, and Vue account for most UI codes. And most backend frameworks for server-side application supports API development just as well.

## Progressive Web App

Another type of web application called Progressive Web App (or PWA) brings the web even closer to the native device experience. PWA runs on user's device's browser but looks and feels just like any native app (most commonly mobile apps on mobile devices).

PWA packs powerful features such as offline capability and push notification, allowing users to install and open just like any other app.

{{< figure src="https://miro.medium.com/v2/resize:fit:1100/format:webp/0*offPX-5HsFy7prMq.png" caption="Progressive Web App examples from Microsoft Edge documentation - Source: https://learn.microsoft.com/en-us/microsoft-edge/progressive-web-apps-chromium/demo-pwas?source=recommendations" >}}

## Conclusion

We have examined the 4 common types of web applications, each with a brief description of their distinct features and underlying technologies.

In the [next lesson](../../03/web-application-development-3-technologies-behind-a-web-application/), we will survey various technologies that make up a web application.

**Previous:** [Web Application Development — #1: What makes a Web Application?](../web-application-development-1-what-makes-a-web-application/)  
**Next:** [Web Application Development — #3: Technologies Behind a Web Application](../../03/web-application-development-3-technologies-behind-a-web-application/)

For Content and Index, visit [Web Application Development — An Introductory course](../web-application-development-introductory-course/)
