--- 
layout: post
title: HTML, CSS and Web Design Rant
tags: 
- CSS
- HTML
- Web
- web design
- XHTML
status: publish
type: post
published: true
author: hleinone
---

I don't know if these are widely accepted, but the following practices in HTML and CSS, or web design in general are pissing me off.

Using font thickness as a hover effect. Not using this should be quite obvious but still I keep seeing it. Jumpy layouts suck, mmkay?

`<br />` all over the place. It's okay when formatting text (poems or such), but do not use it for creating empty space in layout, that's what CSS is for!

CSS `padding` makes me crazy. It's probably one of the easiest ways to get yourself totally lost in how and why some element you've set to occupy 100% of the width, takes some extra space. There's nothing you can do with it then.

Probably the most horrible naming I've seen has been on CSS classes. Naming them `red`, `floatRight`, `witdh100` etc. is reusable, but utterly retarded if you think about it for a while. Separating the layout from the content is the purpose of CSS, now when you've got an element with `class="red floatRight"` it's no different from `color="red" float="right"`. You should not use the elements visual properties in class naming! Common properties that tend to become parts of the class name are darkness variations ie. if in a list every other row has to have a different shade of gray, then try not to name the classes as `some-list-row-dark` and `some-list-row-light`, but instead use the rows' non-visual properties, like odd and even.

Go ahead and post your biggest annoyances in HTML and CSS implementations!
