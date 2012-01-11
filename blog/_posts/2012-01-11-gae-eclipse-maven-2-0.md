---
layout: post
title: GAE + Eclipse + Maven 2.0
tags: 
- AppEngine
- Eclipse
- GAE
- Google App Engine
- Helios
- Java
- Maven
- Maven 2
- mvn
- Programming
status: publish
type: post
published: false
author: hleinone
---

I've [previously posted](http://javanto.com/blog/2010/07/25/gae-eclipse-maven-update-for-helios) about how to set up a Google App Engine project utilizing Maven to its fullest extent. On a recent update to [Google Plugin for Eclipse](http://code.google.com/eclipse/), they introduced [Google Could SQL](http://code.google.com/apis/sql/) which meant changes to the [archetype](https://github.com/javanto/gae-eclipse-maven-archetype) I've created. Also since the last release I had done some other improvements too, so I decided it's time for 2.0!

The POM is huge but it provides as seamless as possible integration of the Maven project to Eclipse. And this is how you use the archetype:

{% highlight bash %}
mvn org.apache.maven.plugins:maven-archetype-plugin:2.2:generate \
  -DarchetypeGroupId=com.javanto \
  -DarchetypeArtifactId=gae-eclipse-maven-archetype \
  -DarchetypeVersion=2.0 \
  -DarchetypeRepository=https://raw.github.com/javanto/repository/releases
{% endhighlight %}

If you, for some reason don't want to use the archetype but rather see an example project, there's [one with pretty much identical content](https://github.com/javanto/gae-eclipse-maven-example). You can see that live in action (when the goddamn spammers haven't flooded the datastore read operations) at [gae-eclipse-maven-example.appspot.com](http://gae-eclipse-maven-example.appspot.com/).

Also, if someone experienced with Eclipse plugin development wants to take the integration a one or two steps further, go ahead and contact me, we should do a m2eclipse plugin!
