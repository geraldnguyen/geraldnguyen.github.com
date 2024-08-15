---
title: "Enable Comment in Hugo site"
date: 2021-10-17T09:19:42+01:00
draft: false
weight: 50
categories: [Tech Related]
tags: [hugo, disqus, comment, doks]
---

*Note 2024-08-15: An earlier version of this site ran on [doks](https://getdoks.org/). I'm in the middle of updating it to a medium-like theme**

-------------

## Introduction

You may have noticed that this website is using a powerful-yet-easy-to-use [doks](https://getdoks.org/) theme.

As I wanted to have more interaction with my readers, enabling comment is a first step.

## Enable Comment in Hugo

Hugo provides built-in support for Disqus. It ships with a `_internal/disqus.html` template that we can simply copy & paste to wherever we want comment to appear. The steps are simple:
- Sign up for a Disqus account, creating a Disqus site
- Configure Disqus' site's shortname in Hugo site configuration. E.g. `disqusShortname = 'yourdiscussshortname'`
- Place Disqus template where appropriate. E.g. `{{ template "_internal/disqus.html" . }}`

More info are available at [https://gohugo.io/content-management/comments/](https://gohugo.io/content-management/comments/)

## Customizing Doks blog template

Back to Doks, however it does not support commenting - at least its Doks child theme version. The `layout` folder contains only a single `index.html` file to customize the home page (e.g. [https://geraldnguyen.github.io/](https://geraldnguyen.github.io/))

Here came the `node_modules/@hyas/doks/layouts` configuration found in `config.toml` file. That module contains all Doks layout files, including the `layouts/blog/single.html` where I need to place `_internal/disqus.html` template. 

Editing `node_modules/@hyas/doks/layouts/blog/single.html` isn't an option - the changes aren't visible to GitHub action or Netlify.

Clone and repointing `layouts` is possible - I haven't tried it though.

The 3rd option is to copy all `node_modules/@hyas/doks/layouts` to site's `layouts` folder, then update the `blog/single.html` with the appropriately placed `{{ template "_internal/disqus.html" . }}`. >> Checkout [the commit](https://github.com/geraldnguyen/geraldnguyen.github.com/commit/8d68bcc1300c76b306939e90bd7132d0943af2f8#diff-5942e866d871d1df37aadd5f735f37d2708ea0c3ebc753ccd1cc38a71a68e19bR13)