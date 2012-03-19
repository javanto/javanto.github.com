---
layout: post
title: Customizing HTML5 Form Validation Error Messages
tags: 
- HTML5
- form
- validation
- error
- message
- JavaScript
- JS
- translate
- popup
- civem.js
status: publish
type: post
published: true
author: hleinone
---

Ever tried out [HTML5 form validation](http://www.w3.org/TR/html5/forms.html#client-side-form-validation)? If not, you should! It'll save server-side resources and improve the usability. Also [all the *good* browsers support it](http://caniuse.com/form-validation). For the not-so-nice browsers, there are [polyfills](https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-browser-Polyfills) for example [H5F](http://www.thecssninja.com/javascript/H5F).

Done with the testing? If so, you might've noticed some issues with the popping up error messages. They may seem ugly (fixing that is not in the scope of this article) and the text of the error message comes out-of-nowhere. It might be even in incorrect language, depending on the one the browser's in. So, whether you want to unify the messages with their server-side equivalents or just force them to match the language of your site, you're in trouble.

Well, there's an [API](http://www.w3.org/TR/html5/association-of-controls-and-forms.html#dom-cva-setcustomvalidity) for setting the error messages.

{% highlight javascript %}
element.oninvalid = function(e) {
  if (!e.target.validity.valid) {
    e.target.setCustomValidity("This field contains errors");
  }
};
element.oninput = function(e) {
  e.target.setCustomValidity("");
};
{% endhighlight %}

But, as you see, you'll end up messing the content with the logic and writing loads of unnecessary JavaScript. The example above doesn't event contain logic for switching between the [various error states](http://dev.w3.org/html5/spec/constraints.html#validitystate). You see where this is going, right?

So, I wrote [civem.js](https://github.com/javanto/civem.js), it stands for **C**ustom **I**nput **V**alidation **E**rror **M**essages. And how do you use it? So simple, [download the latest version](https://github.com/javanto/civem.js/downloads), include it on your page and begin using `data-errormessage` and `data-errormessage-*` attributes on your form elements to validate. You can go check out [the demo on jsFiddle](http://jsfiddle.net/hleinone/njSbH/) or read the [documentation](https://github.com/javanto/civem.js/blob/master/README.md).

{% highlight html %}
<script src="civem-x.x.x.min.js" type="text/javascript">
<input type="email" data-errormessage-value-missing="It says &quot;required&quot; in the HTML!" data-errormessage-type-mismatch="Let me give you a hint: type=&quot;email&quot;." data-errormessage="This is the fallback error message." required>
{% endhighlight %}

Easy, huh!

Now, when most of you readers are busy fiddling with it, is time for some warnings. It hasn't been tested much, it's pre-pre-alpha, so it contain bugs. Also, I'm not (yet) a true JavaScript wizard, so the implementation might just suck. Anyhow, feedback is welcome, so please use the comment facility below and fork button on GitHub!
