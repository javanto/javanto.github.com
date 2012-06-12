--- 
layout: post
title: StackMob with PhoneGap
tags: 
- StackMob
- PhoneGap
- Apache Cordova
- mobile
- HTML5
- JavaScript
- OAuth
- jsOAuth
- REST
status: publish
type: post
published: true
author: hleinone
---

[PhoneGap](http://phonegap.com/) (also known as [Apache Cordova](http://incubator.apache.org/cordova/)), is quite widely known solution to write-once-run-everywhere problem. They solve it in the mobile space by using standard web tools to develop the application. The application is then wrapped in a native one that's basically just a WebKit browser. So, instead of using the native component model you use HTML components.

[StackMob](http://stackmob.com/) is a San Francisco based startup, that gives you a cloud instance and amazingly simple way to create [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) app with a [RESTful](http://en.wikipedia.org/wiki/Representational_state_transfer) JSON web service. You create the API just by specifying the entities and their relations. You could also upload your own custom code and do more complex things, but it won't be covered on this post.

So, I wanted to fiddle with something new and for some reason I came up with these two. But the beginning wasn't that easy. StackMob requires you to authenticate before you can do anything. For Android an iPhone there are SDKs to use, but neither of them would work with PhoneGap easily. Thankfully, they also support [OAuth](http://oauth.net/) authentication. So, I was advised to use [jsOAuth](https://github.com/bytespider/jsOAuth) for authenticating from JavaScript.

When you create a StackMob application, on the App Settings you can find the OAuth keys for both development and production. Depending on the platform you're planning to use, pick the right ones. Then in JavaScript you initialize an `oauth` variable. At this point, I expect you to know your HTML and JavaScript, so I skip the whole setting up of the PhoneGap project. Lookup [here for tutorials](http://wiki.phonegap.com/w/page/35501397/Tutorials) on that matter.

{% highlight js %}
var oauth = OAuth({
  enablePrivilege : false,
  consumerKey : "YOUR_STACKMOB_PUBLIC_KEY",
  consumerSecret : "YOUR_STACKMOB_PRIVATE_KEY"
});
{% endhighlight %}

IMPORTANT NOTICE: Do not use the above method on a true web application, it'll compromise your security, as the users have direct access to your keys from the sources. For web purposes use [StackMob's HTML5 SDK](https://www.stackmob.com/platform/help/tutorials/html5_js_sdk). This is safe though in PhoneGap as the sources are not revealed to the user.

So, now you've got the initial connection to the platform and you can begin using it. For instance, in the StacMob schemas, create an user schema and then an instance with of it in the Test Console. Them we'll try to log him in using JavaScript.

{% highlight js %}
oauth.get(
    "http://your-stackmob-username.mob1.stackmob.com/api/0/your-stackmob-application-name/user/login?username="
         + username + "&password=" + password,
    function(response) {
      alert("Login successful");
    }, function(data) {
      alert("Login unsuccessful");
    });
{% endhighlight %}

Success, right? Well, try creating some other object to the schema and do an insertion through JavaScript. Below's an example how it could be.

{% highlight js %}
oauth.postJSON("http://your-stackmob-username.mob1.stackmob.com/api/0/your-stackmob-application-name/employee",
    {
      name : name,
      title : title
    }, function(data) {
      var id = data.employee_id;
      alert("Created employee with id " + id);
    }, function(data) {
      alert("Employee creation failed");
    });
{% endhighlight %}

From this point on, you're on your own and I think you can manage. It's just a simple JSON API for you to [POST](http://en.wikipedia.org/wiki/POST_%28HTTP%29), [GET](http://en.wikipedia.org/wiki/GET_%28HTTP%29#Request_methods), [PUT](http://en.wikipedia.org/wiki/PUT_%28HTTP%29#Request_methods) or [DELETE](http://en.wikipedia.org/wiki/DELETE_%28HTTP%29#Request_methods).