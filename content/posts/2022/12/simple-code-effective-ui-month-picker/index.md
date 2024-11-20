---
title: Simple Code — Effective UI — Month Picker
subtitle: If you want to let users pick a specific month within a year, what’s the most simple yet effective UI?
date: 2022-12-22T09:19:42+01:00
image: 1_dngIUpZUj098xgpOSI526w.png
draft: false
categories: [Software Development]
tags: [JavaScript, Web Development, UI, Simplicity, Programming, HTML]
medium: https://javascript.plainenglish.io/simple-code-effective-ui-month-picker-4b410a4d0dce
---

# Option 1: separate Month and Year dropdowns

{{< figure src="1_QUOjPpvTDXEzv8HbvpJbNQ.png" caption="Separate month and year dropdowns" >}}

It may be verbose, but you have total control over their placements, label, and format

```html
<select>  
    <option>January</option>  
    <option>February</option>  
    <option>March</option>  
    <option>April</option>  
    <option>May</option>  
    <option>June</option>  
    <option>July</option>  
    <option>August</option>  
    <option>September</option>  
    <option>October</option>  
    <option>November</option>  
    <option>December</option>  
</select> Month  
  
<select>  
    <option>2021</option>  
    <option>2022</option>  
    <option>2023</option>  
</select> Year
```

# Option 2: use the browser’s built-in month picker

{{< figure src="1_dngIUpZUj098xgpOSI526w.png" caption="Built-in month picker" >}}

You have less control over the picker’s appearance, but the code is much cleaner. And localization support is free.

```html
<input type='month'> Month
```

You are able to make some customization. For example, to restrict users to select month-ends of 2020 quarters only

{{< figure src="1_Oc72XbohId4t-8R5Yf36LQ.png" caption="2020 quarters only" >}}

```html
<input type='month' min='2020-03' max='2020-12' step='3'>
```

According to [MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/month):

> The control’s UI varies in general from browser to browser; at the moment support is patchy, with only Chrome/Opera and Edge on desktop — and most modern mobile browser versions — having usable implementations. In browsers that don’t support month inputs, the control degrades gracefully to a simple `<input type=”text”>`, although there may be automatic validation of the entered text to ensure it’s formatted as expected.

As of [December 2022](https://caniuse.com/mdn-html_elements_input_type_month), Firefox and Safari (Desktop) are yet to support this feature, while Chrome-based browsers (including the new Edge from Microsoft) and most mobile browsers (including Safari on iOS) do.

{{< figure src="1_mvG1IQ1HwDjwQ2A1Yf-8dg.png" caption="[https://caniuse.com/mdn-html_elements_input_type_month](https://caniuse.com/mdn-html_elements_input_type_month) (screenshot)" >}}

