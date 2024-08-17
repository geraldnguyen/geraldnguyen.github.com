---
title: "Simple Code - Effective UI - Month Picker"
date: 2021-12-27T09:19:42+01:00
draft: true

tags: [simple code, UI tip, Month Picker]
---

**Note 2024-08-15: An updated version of this article was published in Medium at https://medium.com/javascript-in-plain-english/simple-code-effective-ui-month-picker-4b410a4d0dce?source=user_profile---------78----------------------------**

-----------------

Say you want to obtain a specific month within a year, what's the most effective UI to do so?

## Option 1: separate Month and Year dropdowns

### Sample

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


### Codes

```

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

### Discussion

It may be verbose, but you have total control over their placements, label and format


## Option 2: use browser's built-in month picker

### Sample

<input type='month'> Month

### Codes

```
<input type='month'> Month
```

### Discussion

According to [MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/month), _"The control's UI varies in general from browser to browser; at the moment support is patchy, with only Chrome/Opera and Edge on desktop — and most modern mobile browser versions — having usable implementations"_. As of [December 2021](https://caniuse.com/mdn-html_elements_input_input-month), Firefox and Safari yet to support this feature, while Chrome-based browseres (including the new Edge from Microsoft) do

You have less control over how the picker looks, but the code is a lot cleaner. And the UI is modern enough with free localization support

You are entitled to certain customization. For example, to allow user select a quarters in 2020 only

<input type='month' min='2020-03' max='2020-12' step='3'>

```
<input type='month' min='2020-03' max='2020-12' step='3'>
```