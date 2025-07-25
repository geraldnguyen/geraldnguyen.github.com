---
title: "Web Application Development — #3: Technologies Behind a Web Application"
subtitle: "And what roles each plays"
date: 2023-03-07T12:00:00+07:00
categories: [Programming Tutorials]
tags: [Programming, Web Development, Technology, Beginner, Course, JavaScript, Frontend, Backend]
medium: https://javascript.plainenglish.io/web-application-development-3-technologies-behind-a-web-application-a9e30be2962d
series: "Web Application Development Course"
image: 1_mMUD4mLhl7VskNnvsEN8Dg.jpg
---

*This is part of the [Web Application Development — An Introductory Course](../../02/web-application-development-introductory-course/) series.*

In the [previous lesson](../../02/web-application-development-2-four-types-of-web-applications/), we identified 4 types of web applications: Client-side applications, server-side applications, single-page applications, and progressive web apps. While these applications differ in where and how they operate, their footprints are often found in 3 distinct locations:

• Frontend  
• Network  
• Backend

Most complex web applications need another layer, Database, to store data and users' preferences and activities. Some advanced applications even feed their data to another layer, Data Analytics, to analyze and extract insights.

We will focus on the first 3 in this article and briefly discuss the others.

## Frontend

### Browser

Browsers are the software that enables users to access the web. They also implement or provide runtimes for other web technologies.

While Chrome, Edge, Firefox, and Safari divide the share of browsers on large-screen devices such as laptops or desktops, the share of mobile browsers is mostly a competition between Chrome and Safari on Android and iOS respectively.

{{< figure src="1_mMUD4mLhl7VskNnvsEN8Dg.jpg" caption="Popular browsers" >}}

### HTML

Stand for Hyper Text Markup Language, HTML describes the structure of a web page. A typical web page contains several tags corresponding to headings, paragraphs, links, text, etc… within a `<body>` tag which itself is inside an overall `<html>` tag.

```html
<html>
<head>
  <title>Web Technologies</title>
</head>
<body>
  <h1>Web Application Development - #3: Technologies Behind a Web Application</h1>
  <h2>Frontend</h2>
  <h3>HTML</h3>
  <p>HTML <span class='highlight'>describes</span> the structure...</p>
</body>
</html>
```

### CSS

Stand for Cascading Style Sheets, CSS describes how HTML elements should be displayed on a browser, in a printout, or in other media.

```css
h1 {
  font-size: 40px;
  color: red;
}

h2 {
  font-size: 20px;
}

.highlight {
  background-color: yellow;
}
```

{{< figure src="https://miro.medium.com/v2/resize:fit:720/format:webp/1*XnPyzFocqQNINazagJyMLg.png" caption="Result of the above CSS on earlier HTML" >}}

CSS is quite essential nowadays due to its cosmetic and sometimes animation effects. The below meme captures the reason why HTML and CSS often go together.

{{< figure src="https://miro.medium.com/v2/resize:fit:1100/format:webp/0*b5TWvfbz5RbITuwv.jpg" caption="HTML vs CSS meme showing the difference between structure and styling - Source: https://devhumor.com/media/improving-the-programming-language" >}}

### Javascript

Javascript is _the_ programming language of the Web. It has many use cases such as enabling user interactions, updating content, changing page layout, creating graphics, controlling media…

The latest version of Javascript is ECMAScript 2022. However, ES6 (released in 2015) is still popular. A recent relative of Javascript called TypeScript adds type-checking capability and newer syntax to the language while ensuring it is compatible with the older Javascript versions.

Modern SPA frameworks such as React, Angular, and Vue often extend Javascript by embedding HTML, CSS syntax and adding template constructs to make the language even more powerful and expressive to web developers. Applications built with these frameworks often employs several specialized tools such as webpack, babel to compile, transform, and package HTML, CSS, Javascript, and even images into a few javascript files.

_We will dedicate an entire article about React in a future article. [Stay tuned](https://geraldnguyen.medium.com/subscribe)._

## Network

### Internet, Intranet, LAN, WIFI, 4G, 5G, Fiber…

These are various names of networking technologies that connect a user to a web server.

The time taken for a network signal to travel between 2 points (e.g. user to a web server) is called latency. Bandwidth on the other hand is how many signals (thus how much data) can a network connection transport from one point to the other.

These 2 concepts explain how users can access a web application. The closer the application's server is to a user, the faster it is for the user to access the web application. The higher bandwidth the user and web server's network have, the more content and media-rich the web application can deliver.

### Domain Names, IP addresses

[www.google.com](https://www.google.com/) is the domain name of the Google website while `64.233.170.104` is just one of the many IP addresses Google has. A domain name is the friendly readable address of a web server. Through a hierarchical network of domain name servers (DNS), a browser resolves a domain name to a single IP address which is the exact network address of the web server. That will be the destination to which the browser sends the user's request to access the web application.

```
Pinging www.google.com [64.233.170.104] with 32 bytes of data:
Reply from 64.233.170.104: bytes=32 time=345ms TTL=105
Reply from 64.233.170.104: bytes=32 time=10ms TTL=105
Reply from 64.233.170.104: bytes=32 time=9ms TTL=105
```

### HTTP, HTTPS

Stand for HyperText Transfer Protocol, HTTP is a protocol for transferring textual, image, video, etc… contents over the network. It is probably the most popular protocol on the internet and is the protocol for the world wide web.

HTTPS is the secure version of HTTP validating the web server's identity and encrypting content transfer between the browser and the server. Web applications handling money or containing sensitive information usually require HTTPS with the latest TLS version (TLS 1.3 as of 2023 Feb).

## Backend

### Web Servers

Web servers are a special type of software that receive HTTP requests sent by the browser over the network, process them, and send back HTML, CSS, Javascript, images, etc… as responses.

As of Feb 2023, Apache, Nginx, and Cloudflare are the top 3 most-used web servers according to a survey by [Netcraft](https://news.netcraft.com/archives/2023/02/28/february-2023-web-server-survey.html).

### Application Servers

Application servers are the application codes that receive requests carrying specific instructions at the application level (instead of HTTP or browser level). Application servers are often part of a web server or run behind the web server.

Application servers are often complex and specific to an application or business domain. They are usually responsible for authenticating and authorizing users, implementing business rules and processes, and retrieving or updating databases.

According to [Stack Overflow's Developer 2022 survey](https://survey.stackoverflow.co/2022/#web-frameworks-and-technologies), the most popular programming languages and frameworks to write application server codes are Javascript or Typescript on NodeJS, C# on ASP.NET, Python on Django, FastAPI…

## Other Layers

### Databases

Almost all complex applications retrieve and store user and business data in software called databases. This type of software is specially designed, built, and optimized for durable, fast, efficient, and scalable storage and retrieval of data.

Relational database systems such as Oracle, MS SQL, DB2, MySQL, and PostgreSQL are popular providers of database services. Recently, though, another category of database software called NoSQL is getting more market share.

### Data Analytics

Increasingly, businesses or organizations with tons of data feed them to yet another specialized software to analyze the data and extract insight. Their names often carry "data warehouse", "data lake", "data hub", "analytic", etc… to signify their special purpose.

## Conclusion

We have surveyed various technologies that make up a web application, their roles, and where they operate.

In the [next lesson](../web-application-development-4-terminologies-practices-and-tools/), we will learn the terminologies, practices, and tools of a web application developer.

**Previous:** [Web Application Development — #2: Four Types of Web Applications](../../02/web-application-development-2-four-types-of-web-applications/)  
**Next:** [Web Application Development — #4: Terminologies, Practices, and Tools](../web-application-development-4-terminologies-practices-and-tools/)

For Content and Index, visit [Web Application Development — An Introductory course](../../02/web-application-development-introductory-course/)

