---
layout: post
title:  "CSS Notes from Marksheet.io: Basics and Text"
date:   2015-07-07 10:30:00
categories: css
---

[Marksheet](http://marksheet.io/) has a series of great tutorials on CSS. Here's some notes taken on CSS Basics and CSS Text.

## CSS Syntax

### selector{ property: value }

who{ what: how;}

- the **selector** defines who is targeted, which HTML element(s)
  - Generic tag selectors
    - Just use the tag name, like `a`
  - Classes
    - Just put a dot `.` in front of the class name
  - IDs
    - target it with a hash `#`
  - Hierarchy selectors
    - A space <code>&nbsp;</code> in a selector defines a ancestor/descendant relationship.
  - Pseudo-class selectors
    - HTML elements can have different **states**
    - Pseudo-class selectors are attached to usual selectors and start with a colon :
- the **property** defines what charateristic to alter
- the **value** defines how to alter that charateristic

### When several rules collide

- `#id` selectors are worth 100
- `.class` selectors are worth 10
- `tag` selectors are worth 1

## Color Units
```css
body{ color: black;}
a{ color: orange;}
a{ color: rgb(219, 78, 68);}
a{ color: rgba(219, 78, 68, 0.5);}
a{ color: hsl(4, 68%, 56%);}
body{ color: hsl(4, 68%, 56%, 0.5);}
```

### HSL(HSV)

- the **Hue** a value ranging from 0 to 360, defines which color you want.
- the **Saturation** percentage, ranging from 0% to 100%, defines how much of that color you want.
- the **Lightness** percentag, ranging from 0% to 100%, defines how bright you want that color to be.

## Sizing content and space
- `px` for pixels
- `%` for percentage
- `em` for sizing **relative to the parent’s font-size** value.

Pixels in CSS are straightforward because they define **absolute values**: they are not affected by other inherited CSS properties.
They are also widely used for **positioning** and **spacing** purposes.

Percentages are **relative units**: they rely upon the element’s parent and/or ancestor.

**block-level** elements like paragraphs naturally take up the **whole width available**.

```css
p{ width: 50%;}
```

Percentages can help set other CSS properties, like text size:

```css
strong{ font-size: 150%;}
```

### em
From wikipedia: https://en.wikipedia.org/wiki/Em_(typography)
> An em is a unit in the field of typography, equal to the currently specified point size. For example, one em in a 16-point typeface is 16 points. Therefore, this unit is the same for all typefaces at a given point size.[1]
> Typographic measurements using this unit are frequently expressed in decimal notation (e.g., 0.7 em) or as fractions of 100 or 1000 (e.g. 70/100 em or 700/1000 em).
> The name "em" was originally a reference to the width of the capital "M" in the typeface and size being used, which was often the same as the point size.


em is a relative unit: it depends upon the value of the element’s font-size. (Don’t confuse the em CSS size unit and the em CSS selector, which targets `<em>` HTML elements)

For example, you want your `<h1>` to be twice as big as your body text, your `<h2>` only 1.5 times as big, and your sidebar slightly smaller. This could easily be achieved in CSS:

```css
body{ font-size: 16px;}
h1{ font-size: 2em;}        /* = 32px */
h2{ font-size: 1.5em;}      /* = 24px */
aside{ font-size: 0.75em;}  /* = 12px */
```

### rem
The rem unit is similar to em, but instead of depending upon the parent’s value, it relies upon **the root element’s value**, which is the `<html>` element.

## Reset

### User agent stylesheet
included in the browser and called:

- every time a webpage is rendered
- before any of our CSS is applied

### Apply CSS reset
[http://marksheet.io/css/reset.css](http://marksheet.io/css/reset.css)
CSS reset has been devised to provide a **consistent base** across all browsers.

```html
<head>
  <link rel="stylesheet" type="text/css" href="reset.css">
  <link rel="stylesheet" type="text/css" href="styles.css">
</head>
```

## CSS Font family
Fonts are grouped in 5 generic families:

- serif fonts have small lines attached to the end of each character
- sans-serif
- monospace
- cursive (never used)
- fantasy (never used)
Because the `font-family` property is inherited by all HTML children elements, you can apply a font for the whole HTML document by applying it on the ancestor of all HTML elements: the `<body>` element.

### Web-safe fonts

```css
body{ font-family: Arial;}
```
Your webpage will use Arial provided it is installed on the user’s computer. If the Arial font is not available on the user’s computer, it will use the browser’s default serif font (which is usually Times).

- Arial
- Arial Black
- Comic Sans MS
- Courier New
- Georgia
- Impact
- Times New Roman
- Trebuchet MS
- Verdana

### Apply a list of fonts

```css
body{ font-family: Arial, Verdana, sans-serif;}
```
Look for availability of the fonts the in the order specified. It is good practice to use a **generic family** as the last value.

## CSS font properties

### font-size
set font size: `p{ font-size: 16px;}` (CSS size units)

### font-style
set font style: `h2{ font-style: italic;}`

- font-style
  - normal
  - oblique (never used)

### font-weight

```css
font-weight: 100; /* Thin */
font-weight: 200; /* Extra Light */
font-weight: 300; /* Light */
font-weight: 400; /* Which is like font-weight: normal; */
font-weight: 500; /* Medium */
font-weight: 600; /* Semi Bold */
font-weight: 700; /* Which is like font-weight: bold; */
font-weight: 800; /* Extra Bold */
font-weight: 900; /* Ultra Bold */
```

###font-variant

```css
font-variant: normal;
font-variant: small-caps;
```

## Line Height

- line-height units:
  - px
  - em
  - %
  - unitless numbers, like 1.5

### Why line-height?
- define a readable line spacing
- Because readibility is dependent upon the size of the text, it is recommended to use a dynamic value that is relative to the size of the text. the recommended method is unitless numbers:
  - for body text, a line height of 1.5 times the size of the text is recommended.
  - for headings, a line height of 1.2 is recommended

### Line-height inheritance
`line-height` is inherited by the child elements, it will remain consistent no matter what `font-size` is subsequently applied.

### CSS font shorthand
- font property regroups (in order)
  - font-style
  - font-variant
  - font-weight
  - font-size
  - line-height
  - font-family

Define 6 properties through single one: `body{ font: italic small-caps bold 16px/1.5 Arial, sans-serif;}`
There must be a slash `/` between the font-size and the line-height. Only the font-size and font-family are mandatory. If you’ve previously define one of the font properties and use the font shorthand afterwards, it will override the previously defined values.

Other shorthand properties exist, like `background`, `border` and `margin`.

## CSS text properties

- text-align
  - left
  - right
  - center
  - justify (not recommended because of lack of readability) (text justification)
- text-decoration
  - <span style="text-decoration:overline">overline</span>
  - <span style="text-decoration:underline">underline</span>
    - By default, HTML links (`<a>`) have a `text-decoration: underline;` applied to them.
      - One of the first things coders usually do is to remove this default styling: `a{ text-decoration: none;}`
  - <span style="text-decoration:line-through">line-through</span>
- text-indent
  - The text-indent property allows to add space before the **first letter of the first line** of a block-level element.
  - Only the first line is indented. If you want to offset the whole block of text, use paddings.
- text-shadow
  - x the horizontal offset
  - y the vertical offset
  - the blur
  - the color
  - <span style="text-shadow: 0 2px 5px rgba(0,0,0,0.5);"> span{ text-shadow: 0 2px 5px rgba(0,0,0,0.5);} </span>
