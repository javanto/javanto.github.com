--- 
layout: post
title: "HOW-TO: GAE + Eclipse + Maven"
tags: 
- App Engine
- AppEngine
- archetype
- Eclipse
- GAE
- Google App Engine
- Guestbook
- Java
- Maven
- Maven 2
- Programming
status: publish
type: post
published: true
author: hleinone
---

**Help me in making the integration of GAE, Eclipse and Maven smoother and [star this issue](http://code.google.com/p/googleappengine/issues/detail?id=5042) in AppEngine**

**Update: This solution doesn't work on Eclipse 3.6 Helios and newest version of m2eclipse, check out [the new solution](/blog/2010/01/26/how-to-gae-eclipse-maven) if you're on the newer version.**

Nowadays everybody wants to manage their Java projects with [Maven](http://maven.apache.org/). And that's totally sensible since Maven makes some things pretty damn easy, mostly because there's a plugin for about everything. Yes, there is a [plugin](http://www.kindleit.net/maven_gae_plugin/index.html) for [Google App Engine](http://code.google.com/appengine/) (GAE) too, with which you can run the local development server and deploy your app. The same features are available on the [Google Plugin for Eclipse](http://code.google.com/eclipse/), which is the Google preferred way to develop Java-based App Engine applications. So of course you'd like to also utilize the IDE integration, especially when the startup time of the local GAE development server is significally faster, thus increasing your productivity. So far, the problem has been, how to enjoy the benefits of both worlds, and I've have to tell you, it is now solved. Another thing I have to tell you is, how, so here we go.

**Requirements**

* [Google App Engine SDK for Java 1.3.0](http://code.google.com/appengine/downloads.html#Google_App_Engine_SDK_for_Java)
* [Eclipse == 3.5 (with Java Development Tools)](http://www.eclipse.org/downloads/)
* [Maven >= 2.0.9](http://maven.apache.org/download.html)
* [Google Plugin for Eclipse (link to the update site)](http://dl.google.com/eclipse/plugin/3.5)
* [m2eclipse (link to the update site)](http://m2eclipse.sonatype.org/sites/m2e)

The integration isn't compatible with m2eclipse, but it's required for attaching the Maven `war:exploded` goal to Eclipse compilation. Also the integration is not perfectly production ready since it has to rely on a snapshot version of [maven-eclipse-plugin](http://maven.apache.org/plugins/maven-eclipse-plugin/) hosted at [Bearded Geeks' Maven repository](http://beardedgeeks.googlecode.com/svn/repository/releases/). It's required for linkedResource configuration and using the Eclipse Java Compiler. For more details see plugin's issue tracker for [linkedResources](http://jira.codehaus.org/browse/MECLIPSE-402) and [output classes](http://jira.codehaus.org/browse/MECLIPSE-422) (Voting on the issues and by other means trying to get the patches in will be highly appreciated).

So, I've converted the [Guestbook Example](http://code.google.com/appengine/docs/java/gettingstarted/) into a Maven managed one and I'm distributing it as a [Maven archetype](http://maven.apache.org/guides/introduction/introduction-to-archetypes.html). So, here's how you begin:

{% highlight bash %}
mvn org.apache.maven.plugins:maven-archetype-plugin:2.0-alpha-4:generate \
  -DarchetypeGroupId=org.beardedgeeks \
  -DarchetypeArtifactId=gae-eclipse-maven-archetype \
  -DarchetypeVersion=1.1.2 \
  -DarchetypeRepository=http://beardedgeeks.googlecode.com/svn/repository/releases
{% endhighlight %}

By running that command you can create a Maven managed GAE app of your own. Just remember to change everything to match your application, like appId etc. To enable Eclipse integration run `mvn eclipse:eclipse`. After that you'll need to import the project to Eclipse and after that and few project refreshs you should be fine (don't mind it giving warnings about some missing libraries, they shouldn't affect). Just run it as you usually run App Engine projects in Eclipse. Also on every Eclipse startup the Eclipse might put its GAE libraries back to `war/WEB-INF/lib` which'll cause failure on startup, but cleaning the project solves the problem. You can also run your project using maven-gae-plugin by typing `mvn gae:run` on command line or deploy to App Engine by typing `mvn gae:deploy`.

If you're not that into archetypes, you can find the project sources through SVN at [Bearded Geeks' Google Code project](http://code.google.com/p/beardedgeeks/source/browse/#svn/gae-eclipse-maven-example). The guestbook example can be found up and running at the [project's GAE page](http://gae-eclipse-maven-example.appspot.com/).
