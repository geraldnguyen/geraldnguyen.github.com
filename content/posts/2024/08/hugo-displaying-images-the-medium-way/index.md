---
title: "Hugo - Displaying Images the Medium way"
subtitle: Markdown, Figure, Multi-figures ways
date: 2024-08-26T09:19:42+01:00
image: 1_UEaVZcIzkXkW0N44KUeW5A.jpg
draft: false
categories: [Tech Related]
tags: [Medium, Hugo, Image, Markdown, Shortcode]
---

*Please ignore the `\` in front of `{` in code examples. It is used to prevent Hugo from processing the shortcodes even in code block*

# The Markdown way

```
![alt title](1_nHC2UMz-xJM-lvPPwSh_vQ.webp)
```

![title](1_nHC2UMz-xJM-lvPPwSh_vQ.webp)

Though it is recommended, the `alt-title` is optional. The above can be expressed as `![alt title](path-to-image.jpg)`

**Pros:** 

*   Simple
*   Natively markdown

**Cons:**

*   No caption
*   Center alignment not guaranteed

# The Medium way

```
{{\< figure src="1_nHC2UMz-xJM-lvPPwSh_vQ.webp" caption="optional caption" \>}}
```

{{< figure src="1_nHC2UMz-xJM-lvPPwSh_vQ.webp" caption="optional caption" >}}

**Pros:**

*   Support caption, plain text and markdown
*   Center alignment

**Cons:**

*   Not native to markdown
*   Extra shortcode syntax

# Supporting Medium's multiple figures display

We'll use the following shortcode to display multiple images the same way Medium does

```html
# multi-figures.html

<div class="multi-figures d-block">
  <style>
    .multi-figures figure {
      margin: 0 0.5rem;
    }

    .multi-figures-caption {
      grid-area: caption;
      text-align: center;
    }

  </style>
  <div class="multi-figures-area d-flex flex-row justify-content-center">
    {{ .Inner }}
  </div>

  {{ with (.Get "caption") }}
    <p class="multi-figures-caption figcaption">{{ . }}</p>
  {{ end }}
</div>
```

## A single caption for all images

```
\{\{< multi-figures caption="a single caption for 2 images" >}}

\{\{< figure src="1_nHC2UMz-xJM-lvPPwSh_vQ.webp" >}}
\{\{< figure src="1_UEaVZcIzkXkW0N44KUeW5A.webp" >}}

\{\{</ multi-figures >}}
```

Set the `caption` attribute to the `multi-figures` shortcode. The caption will display across all images embedded within that shortcode.

{{< multi-figures caption="a single caption for 2 images" >}}

{{< figure src="1_nHC2UMz-xJM-lvPPwSh_vQ.webp" >}}
{{< figure src="1_UEaVZcIzkXkW0N44KUeW5A.webp" >}}

{{</ multi-figures >}}



## Or a caption for each image

```
\{\{< multi-figures >}}  
  
\{\{< figure src="image-1.jpg" caption="or a caption" >}}  
\{\{< figure src="image-2.jpg" caption="for each" >}}  
  
\{\{</ multi-figures >}}
```

Set the `caption` attribute in each `figure` shortcode to display a caption under each image

{{< multi-figures >}}

{{< figure src="1_nHC2UMz-xJM-lvPPwSh_vQ.webp" caption="or a caption" >}}
{{< figure src="1_UEaVZcIzkXkW0N44KUeW5A.webp" caption="for each" >}}

{{</ multi-figures >}}