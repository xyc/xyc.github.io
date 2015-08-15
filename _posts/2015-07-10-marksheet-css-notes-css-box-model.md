---
layout: post
title:  "CSS Notes from Marksheet.io: CSS Box Model"
date:   2015-07-10 10:30:00
categories: css
---

[Marksheet](http://marksheet.io/) has a series of great tutorials on CSS. Here's some notes taken on CSS Box Model.

## CSS background

### background-color

Default value: `transparent`, not inherited by descendants.

The whole element will be filled with a **plain** background color.

### background-image
```css
body{ background-image: url(images/diagonal-pattern.png);}
```

### `<img>` vs CSS background images

- `<img>` is for images that are part of the **content**
	- examples: logo, thumbnail of gallery, picture of product
- CSS background images are purely **decorative**
	- examples: diagonal pattern, landscape, cart icon

### Gradients

- linear-gradient
- radial-gradient
background graidents are **background images**.
```css
body{ background-mage: linear-gradient(white, gray);}
```

### background-position
`background-position: x y`

- pixel values `px`
- percentages (relative to HTML element's dimensions)
- keywords `center`, `left`, `right`, `bottom`, `up`
```css
body{ background-position: right bottom;}
```

### background-repeat
```css
/* default repeat indefinitely */
body{ background-repeat: repeat-x;} /* Only horizontally */
body{ background-repeat: repeat-y;} /* Only vertically */
body{ background-repeat: no-repeat;} /* The background image will only appear once */
```

## CSS display

### display: block

- block-level elements
- inline elements
- list-item
- table-cell

This will turn any element into a **block** element.

This technique is often used on **links** in order to increase their clickable zone, which can be easily evaluated by setting a background color.

### display: inline
This turns any element into **inline** elements, as if they were just simple text.

It’s often used to create **horizontal navigations**, where list items are semantically but not visually useful. (need to display list items inline)

### display: list-item
HTML elements that default displays list-item: `<li>`, `<dd>`

A list item is rendered with a bullet point (if in an unordered list `<ul>`) or with a incremental number (if within an ordered list `<ol>`).

Because the rendering of these bullet points and numbers varies across browsers, and is also hard to style in CSS, the `display: list-item` rule is never used. Actually, it is common for `<li>`s to be rendered as `display: block` or `display: inline`, as they are more flexible to style.

### display: none
Applying `display: none;` to an HTML element make it disappear on the webpage

### visibility: hidden
Applying `visibility: hidden;` hides an element from your page, but it still **takes up the space** it was supposed to.

## CSS height and width

The dimensions (or height and width) of an element are **dynamic**, as they fluctuate in order to fit the content. It is somehow possible to set **specific** dimensions.

### Setting both height and width
```css
blockquote{ background: yellow; height: 50px; width: 100px;}
```

### CSS overflow
The `overflow` CSS property allows us to manage the case of content being longer than its container.

- `overflow: visible`: the content will be displayed anyway, because “Why would you want to prevent content from being read by the user if it’s present in the code?”
- `overflow: hidden`: forbid any overflowing content to be seen.
- `overflow: scroll`: display scrollbar
- `overflow: scroll`: scrollbar appear only when overflow

### fixed dimensions
Applying specific dimensions are often required for a design to look visually appealing but can have unintended consequences. In that regard:

- make sure your content doesn’t overflow
- if it does, use `overflow: hidden` or `overflow: auto` to prevent your design from breaking

## CSS border
- `border-color` defined by using a color unit
- `border-style` can be solid, dashed, dotted…
- `border-width` defined by using a size unit

possible sides:

- `border-top`
- `border-bottom`
- `border-left`
- `border-right`

The shorthand property border allows to define all 3 properties at once: `blockquote{ border: 1px solid yellow;}`

### Single border
```css
blockquote{ border-bottom-color: yellow; border-bottom-style: solid; border-bottom-width: 1px;}
```

### Shorthand combinations
You’ll usually end up using the 5 shorthand versions only:

- `border`
- `border-top`
- `border-bottom`
- `border-left`
- `border-right`

## CSS padding

The padding is the space between an element’s border and its content. The amount of space can be defined using any of the size units. As for borders, the padding can be set individually for any of the 4 sides.

Because the padding lies **between** the border and the content, it’s easier to visualize the inner space with a border applied.

## CSS margin
If padding adds space *inside* an element (between its border and its content), margins adds space *outside* between an element and other elements. (push away neighbors)

```css
.title{ margin-bottom: 30px;}
.subtitle{ margin-top: 15px;}
```

```html
<h1 class="title">MarkSheet</h1>
<h2 class="subtitle">A simple HTML/CSS tutorial</h2>
```

As the title of this section may have hinted at the answer, the margin between the two elements will be 30px, and not 45px. That is because margins that “touch” each other will **merge** with each other. You can consider an element’s margin as the **minimum space** it want to stay away from other elements.

### choose between margin and padding
If there's no border, since margins and paddings are transparent, the result will look the same. Ask yourself why you’re spacing things.
- Is it to allow the inner content to breath more? -> padding
- Or is to allow the whole element to breath more? -> margin
and, consider how margins can merge.

## CSS size shorthand wheel

- 2 values: `selector: { margin|padding|border-width: top|bottom right|left }`
- 3 values: `selector: { margin|padding|border-width: top bottom right }`
- 4 values: `selector: { margin|padding|border-width: top right bottom left }`

### remember the order
- if you enter 2 values (top/right), you omit setting bottom and right. Because bottom is the vertical counterpart of top, it will use top’s value. And because left is the horizontal counterpart of right, it will use right’s value.
- if you enter 3 values (top/right/bottom), you omit setting left. As right’s counterpart, it will use its value.

> “Border is a shorthand property that includes border-width which is another shorthand property”

`selector: { border: (border-width)? (border-style) (border-color)}`
border-width in border shorthand is omittable.

# CSS positioning

## CSS the flow
without CSS, HTML already has its rules:

- fluidity: how the content adapts to browser dimensions
	- whole width
	- word wrap
	- auto height
- ordering: in which order elements appear
- stacking: how elements appear on top of each other
This natural behavior is **logical**

### Fluidity
**content** is King
All `block` elements are **fluid**. They will naturally adapt their layout to accommodate their inner content:

- **width: 100%**
	- a block will take up the whole width available
- **word wrap**
	- if a block’s inline content doesn’t fit on a single line, it will continue on a new line
- **height: auto**
	- a block’s height varies automatically to match its content’s size

### Ordering
HTML elements are displayed in the **order** that's written **in the code**.

### Stacking
Browser has **3 dimensions**. Each HTML element belongs to an imaginary **layer**. The stack order depends on how elements are nested: child elements appear **on top** of their respective parents.

- Each **nested** element appears **on top of** its parent.
- The **deeper** in the hierarchy, the higher in the stack.

### Breaking the flow
- disrupt the flow
	- fluidity
		- `height` and `width` can alter an element’s fluidity
	- stacking
		- `z-index` can alter the order in which elements are stacked
	- `float` disrupts an element’s behavior as well as its surroundings
	- `position` `absolute` and `fixed` remove an element from the Flow


## CSS position
`selector { position: static|relative|absolute|fixed; left:...; right:...; ... }
oftehn used *alongside* 4 **coordinates** properties:

- `left`
- `right`
- `top`
- `bottom`

### static
This is the **default** position value: static elements just follow the natural flow. They aren’t affected by any left, right, top or bottom value.

### relative
When the `position` is set to **relative**, an element can move according to its **current position**.

**All the other elements don’t know that element has moved**.

### absolute
When the position is set to absolute, an element can move according to the **first positioned ancestor**.

A **positioned** element is one whose `position` value is either `relative`, `absolute` or `fixed`. So unless the position is not set or `static`, an element is positioned.

[Example](http://marksheet.io/css-position.html#absolute)

The yellow section has a height of 200px and its position set to relative, which turns it into a point of reference for all my child elements.

As the green paragraph’s position is set to absolute, it can freely move according to the yellow section. By setting both bottom and left values, it will move from the bottom left corner.

What happens if we set both left AND right?

- If the `width` is not set, applying `left: 0` and `right: 0` will **stretch the element across the whole width**. It’s the equivalent of setting `left: 0` and `width: 100%`.

- If the `width` is set, then the `right` value is discarded.

### fixed
When the `position` is set to **fixed**, it acts like **absolute**: you can set left/right and top/bottom coordinates.

The only difference is that the **point of reference** is the viewport. It means that a fixed element won’t ***scroll*** with the page; it is ***fixed*** on the screen.


## [CSS float](http://marksheet.io/css-float-clear.html)

float is probably the most difficult CSS concept to grasp. Its behavior can be intriguing, unexpected, and magical. Probably because, of all positioning properties there are, it is the one that most influences its **surroundings**.

In other words, applying a float not only modifies the element it’s applied upon but also alters its ancestors, siblings, descendants, and following **elements**.

`float` can have 3 values:

- `left` and `right` turns element into **floating** one
- `none` removes

### When to use float
The purpose of **floating** an element is to **push it to one side** and make the text **wrap around it**.

Because `float: left` **takes the image out of the flow**, the yellow paragraph’s height is only **the height of its text**. In other words, the height of the image isn’t taken into account.

### float = block
Floating elements will have a `display: block` applied to them automatically, and will mostly behave like blocks:
- you can set a specific `height` and `width`
- if no height is set, the element’s height is that of the `line-height` (in the example height is image's height)
- if a `width: 100%` is applied, it will look like a block-level element

### Clearing the float
The `clear` property allows to **push elements** after **the float**. It can only be applied on **block** elements.

[Example](http://marksheet.io/css-float-clear.html#clearing-the-float)

Instead of having the text pushed next to the image (like the first example, no float), the `clear: left` pushes the text **below** the image. It’s different from having no float or clear at all, as the image is on its own line and not on the same line as the text.
