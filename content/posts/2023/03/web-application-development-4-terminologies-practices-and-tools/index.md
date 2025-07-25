---
title: "Web Application Development — #4: Terminologies, Practices, and Tools"
subtitle: "The basics of Agile development, team composition, and what tool we need for the remaining of this course"
date: 2023-03-28T12:00:00+07:00
categories: [Programming Tutorials]
tags: [Programming, Web Development, Software Development, Agile, Course, Team Management, Development Tools]
medium: https://javascript.plainenglish.io/web-application-development-terminologies-practices-and-tools-db4a743bba5b
series: "Web Application Development Course"
image: 0_JKjcGKvX0TwX3TTs.jpg
---

*This is part of the [Web Application Development — An Introductory Course](../../02/web-application-development-introductory-course/) series.*

## Agile Software Development

### History: The Waterfall model

For most of its history, the software development communities followed the Waterfall Development Model which is a sequential process of distinct stages, each must finish before the next stage can begin.

{{< figure src="https://miro.medium.com/v2/resize:fit:1100/format:webp/0*2GwuUp8N5HGjF-eU" caption="The Waterfall Model — Source: https://commons.wikimedia.org/wiki/File:Waterfall_model_%281%29.svg" >}}

The Waterfall model is a structured approach emphasizing requirement collection, analysis, and software design. It is however not flexible enough in today's fast-changing environment in which requirements, user needs, and technologies change every few months, even weeks.

### Agile Development Model

Came the agile development model where:

> **[Individuals and interactions]** over processes and tools  
> **[Working software]** over comprehensive documentation  
> **[Customer collaboration]** over contract negotiation  
> **[Responding to change]** over following a plan
> 
> That is, while there is value in the items on the right, we value the items on the left more
> 
> — [https://agilemanifesto.org](https://agilemanifesto.org/)

Guided by the above values from the Agile Manifesto, the Agile development model has taken over the software development community. Even though the exact practice of each agile development team may differ, they all share the following characteristics:

• **Iterative, incremental, and evolutionary**: each development project comprises multiple iterations, and each has a small, deliverable scope which may be a new feature or an improvement of earlier features. Changes are considered and accommodated during these iterations as and when they occur.

• **Frequent and open communication**: Agile practices emphasize frequent communication among members of the development team and between the team and business users (or their representatives). The preference is face-to-face communication and thus co-location of users and the team.

• **Very short feedback loop and adaptation cycle**: Each iteration lasts one or a few weeks rather than months or years as in the case of the waterfall model. That enables fast and early feedback and better response to changes. The Agile development team conduct short daily stand-up where each team member updates the team about his/her progress, plan for the day, and if there is any blocker for their tasks.

• **Quality focus**: Agile team adopts modern practices that automate software testing, continuous integration, continuous deployment… After all, working software is the true measure of success.

{{< figure src="https://miro.medium.com/v2/resize:fit:750/format:webp/0*JKjcGKvX0TwX3TTs.png" caption="Keeping Daily Stand-up short — source: https://medium.com/@stevoscript/why-your-team-should-try-asynchronous-daily-stand-ups-87f1b809e5c8" >}}

The two popular Agile methodologies are Scrum and Kanban. In practice, teams often tweak the prescribed methodologies guidelines to fit their constraints and preferences. Some incorporate other techniques such as Story point, BDD, TDD, Pair programming, CI/CD, DevOps, Burn-Down, Velocity, etc… into their agile working model.

## Composition of a Development Team

### Developers

Developers are the people who write codes. They have many names (such as programmer, coder, dev, and software engineer) and can specialize in one or multiple domains (such as database, backend, web, mobile, and DevOps). Those that reach expert level can perform design and architect tasks and thus carry the architect job title.

{{< figure src="https://miro.medium.com/v2/resize:fit:720/format:webp/0*mJNSNlXjB5oRCeQO" caption="Photo by Kelly Sikkema on Unsplash" >}}

### Testers

Testers are also known as Quality analysts or just QA. They are the people who validate the written codes (at component, service levels, or on a working software) against established functional and non-functional requirements.

The validation can be manual, performed by human hand, mouse clicks, and eyes. This is how most software testings have been performed, until recently.

Or it can be automated by special test automation software. In this aspect, the testers also write codes to describe their test scenarios. This blurs the lines between developer and tester.

{{< figure src="https://miro.medium.com/v2/resize:fit:720/format:webp/0*bgqvvEaB2nVzPDy4" caption="Photo by Tim van der Kuip on Unsplash" >}}

### Product Managers

Product Managers are the people who represent users and define the product's features that the team is going to develop. They also prioritize the list of features, often broken down into smaller units called stories, against the team's resources, availability, and other types of constraints (e.g. market and regulation constraints, and organization directives).

{{< figure src="https://miro.medium.com/v2/resize:fit:720/format:webp/0*nVwHhjpzrbx7ku-n" caption="Photo by Jason Goodman on Unsplash" >}}

### Project Manager

A development team usually has only 1 project manager who helps the team with project activities (e.g. project planning, status tracking) and keeps the project going (e.g. removing blockers, identifying and managing risk affecting project delivery).

Scrum methodology, although there is no project manager, defines a Scrum Master role which is very similar to that of a traditional project manager. The planning and status tracking, however, is part of the Product Manager's responsibilities.

{{< figure src="https://miro.medium.com/v2/resize:fit:720/format:webp/0*hsOtfp3_r6V4yr8e" caption="Photo by Mapbox on Unsplash" >}}

## Essential Tools for Web Development

### [NodeJS](https://nodejs.org/en)

We will use JavaScript as the programming language for both the frontend and backend development. This will ease your learning curve by reducing the number of programming languages you need to learn.

NodeJS provides the runtime for backend development in Javascript language. It also enables a host of Javascript-based tools for front-end development. We shall elaborate more on this in future lessons.

### [Visual Studio Code](https://code.visualstudio.com/)

Visual Studio Code is a free, lightweight, yet powerful code editor by Microsoft. For Javascript-related projects, VSCode is my first choice.

Both built-in capabilities and plug-in available in VSCode can elevate its status from a code editor to close to an IDE (Integrated Development Environment).

### [Git](https://git-scm.com/)

Version Control is a must nowadays. And Git is the most popular version control software.

Version Control allows developers to keep track of changes made and revert to a past version if necessary.

### Chrome

We need Chrome to actually test and debug the web application we are about to develop.

We especially need [Chrome's developer tools](https://developer.chrome.com/docs/devtools/) to debug the codes.

## Conclusion

We have gone through the basics of Agile software development, learned a few terms, known a few job roles, and familiarized ourselves with a few tools which we will use in future lessons.

In the next lesson, we will start with some actual coding by practising the basics of HTML and CSS.

**Previous:** [Web Application Development — #3: Technologies Behind a Web Application](../web-application-development-3-technologies-behind-a-web-application/)  
**Next:** Frontend Development: HTML, CSS, Responsive layout, Javascript (Coming Soon)

For Content and Index, visit [Web Application Development — An Introductory course](../../02/web-application-development-introductory-course/)