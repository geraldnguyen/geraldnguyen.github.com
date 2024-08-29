---
title: Hugo - Displaying Quotes the Medium styles
subtitle: Learn the Block quote and Pull quote ways
date: 2024-08-29T09:19:42+01:00
image: extra-pull-quote.png
draft: false
categories: [Tech Related]
tags: [Medium, Hugo, Image, Markdown, Shortcode]
---

*Please ignore the `\` in front of `{` in code examples. It is used to prevent Hugo from processing the shortcodes even in code block*

# The Block Quote

> This is a block quote

```
> This is a block quote
```

# The Pull Quote

We need a custom quote.html shortcode and some custom CSS

**The `quote` shortcode**:

```html
# quote.html

{{$quote := .Get 0}}

<blockquote class="medium-quote">
  <p>
    {{ markdownify $quote }}
    {{ markdownify .Inner }}
  </p>
</blockquote>
```

**The CSS**:

```html
# css - Pardon the css. I haven't got to the clean-up part

blockquote.medium-quote {
  -webkit-text-size-adjust: 100%;
  --reach-tabs: 1;
  --reach-menu-button: 1;
  text-rendering: optimizeLegibility;
  -webkit-font-smoothing: antialiased;
  color: rgba(0,0,0,0.8);
  font-family: medium-content-sans-serif-font, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  font-weight: 400;
  word-break: break-word;
  word-wrap: break-word;
  box-sizing: inherit;
  margin: 0;
  padding-left: 30px;
  box-shadow: unset;
}

.medium-quote p {
  -webkit-text-size-adjust: 100%;
  --reach-tabs: 1;
  --reach-menu-button: 1;
  text-rendering: optimizeLegibility;
  -webkit-font-smoothing: antialiased;
  word-break: break-word;
  word-wrap: break-word;
  box-sizing: inherit;
  margin: 0;
  font-family: sohne, "Helvetica Neue", Helvetica, Arial, sans-serif;
  color: #6B6B6B;
  font-style: normal;
  margin-bottom: -0.46em;
  line-height: 40px;
  letter-spacing: -0.009em;
  font-weight: 300;
  font-size: 28px;
  margin-top: 1.75em;
}
```

## Simple text

{{< quote "This is a pull quote" />}}

```
\{\{< quote "This is a pull quote" />}}
```

## Complex paragraph

{{< quote >}}

This is a 

multi-line quotes

{{< /quote >}}


```
\{\{< quote >}}

This is a 

multi-line quotes

\{\{< /quote >}}
```

# The Extra-Pull Quote

Our `quote` shortcode implementation allows for more content formattin optiong within the pull quote.

For example:

{{< quote >}}

You can even have 
> quote within quote

![](1_fXMtjoukeOudyhZ1fJolKQ.jpg)

Even image!
{{< /quote >}}


```
\{\{< quote >}}

You can even have 
> quote within quote

![](1_fXMtjoukeOudyhZ1fJolKQ.jpg)

Even image!

\{\{< /quote >}}
```