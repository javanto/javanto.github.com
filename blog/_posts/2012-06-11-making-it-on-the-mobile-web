---
layout: post
title: Making It on the Mobile Web
tags: 
- HTML5
- CSS3
- mobile
- tablet
- desktop
- media-query
- fluid
- layout
status: publish
type: post
published: true
author: hleinone
---

Yet noticed that this site works on both Android and iOS devices? How was this all achieved? Well, first-of-all you should know about [HTML 5 Boilerplate](http://html5boilerplate.com/) a great basis for any website. There's also [Mobile Boilerplate](http://html5boilerplate.com/mobile) that I used as well, combining things from both that I thought were important. The two HTML templates get you pretty far in supporting various screen sizes and they're also well documented.

From the JavaScript side both boilerplates utilize [Modernizr](http://modernizr.com/) which is an awesome library for feature detection and also includes [YepNope](http://yepnopejs.com/). Them combined with [ClassIE](https://github.com/pyrsmk/ClassIE) make loading of JavaScript libraries very efficient, you'll have full control of the sequence when they're to be loaded.

The boilerplates have a slight dependency on [jQuery](http://jquery.com/) which I wanted to ditch and move to a more light-weight [Xui](http://xuijs.com/). Xui has separate IE and non-IE versions, loading the correct one with YepNope and ClassIE was super-easy.

I still want to mention Mobile Boilerplate's helper.js. A library which contains various fixes for mobile devices. Setting them up is easy, first detecting if we're on a touch device and then applying the fix by calling the function. With the helper.js you can easily remove the address bar from iOS and Android browsers, giving a bit more space for your relevant content.

One thing is still to be mentioned, the CSS. CSS is probably the biggest factor in the whole [mobile first](http://www.lukew.com/presos/preso.asp?26) and [responsive web design](http://www.alistapart.com/articles/responsive-web-design/) approaches. In the boilerplates the base CSS is done so that it looks fine on a small screen, then using [CSS media-queries](http://www.w3.org/TR/css3-mediaqueries/) to make changes to the layout when the browser resolution increases. I defined limits on certain absolute resolution width points to make it look different on tablet and desktop displays. 

A bit more difficult part in the whole responsiveness are the HTML elements that need to be responsive, like the img tag for example. For a larger display you may want to load a larger image. There's really no solution that is both able to run without JavaScript and doesn't do unnecessary requests. I decided to use [Molt](https://github.com/pyrsmk/molt) for its small size, dependency freeness and nice syntax. So using just regular img tags, I define a `data-url` attribute that maps the window width to file names. The only compromise is using a different attribute on the img tag, which is in my opinion better than the alternative solutions.

Allright, that's about it. You can see the full source of this website on [GitHub](https://github.com/javanto/javanto.github.com).
