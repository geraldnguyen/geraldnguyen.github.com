---
title: "Using Bootstrap with React"
date: 2020-04-20T09:19:42+01:00
subtitle: "Because both are great!"
draft: false
weight: 50
tags: [react, bootstrap, programming, javascript]
---

## Mixing browser and React

Insert links to Bootstrap's javascript and CSS and its dependencies in app's `index.html`

```
<head>
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
</head>

<body>

  <!-- other content -->
  
  <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js" integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6" crossorigin="anonymous"></script>

  <script src="https://code.jquery.com/jquery-3.4.1.slim.min.js" integrity="sha384-J6qa4849blE2+poT4WnyKhv5vZF5SrPo0iEjwBvKU7imGFAV0wwj1yYfoRSJoZ+n" crossorigin="anonymous"></script>
  <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
</body>
```

Invoke appropriate method via `window.$` jQuery object

```
window.$('#about-modal').modal()
```

## Pure React way

Install Bootstrap as a NPM module

```
npm install bootstrap --save
npm install jquery popper.js --save 
```

Load bootstrap CSS and Javascript in React app's entry point (e.g. `index.jsx` or `app.jsx`)

```
import 'bootstrap/dist/css/bootstrap.min.css';
import 'bootstrap';
```

Obtain `$` jQuery object and invoke appropriate method

```
import $ from 'jquery';
...
$('#sync-modal').modal(),

```

## Reference
- https://getbootstrap.com/docs/4.4/getting-started/webpack/