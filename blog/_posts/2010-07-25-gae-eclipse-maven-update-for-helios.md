--- 
layout: post
title: GAE + Eclipse + Maven - Update for Helios
tags: 
- "3.6"
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
published: true
author: hleinone
---

**Help me in making the integration of GAE, Eclipse and Maven smoother and [star this issue](http://code.google.com/p/googleappengine/issues/detail?id=5042) in AppEngine**

You may have noticed that [Eclipse 3.6 aka Helios is out](http://www.eclipse.org/org/press-release/20100623_heliosrelease.php). That meant some changes on the m2eclipse plugin, mainly [removal of the external tools Maven builder](https://issues.sonatype.org/browse/MNGECLIPSE-2245), which made [the previous GAE + Eclipse + Maven configuration](/blog/2010/01/26/how-to-gae-eclipse-maven/) unusable.

Sounds horrible? It shouldn't, since I've fixed the archetype and the [example project](http://beardedgeeks.googlecode.com/svn/gae-eclipse-maven-example/trunk/) to be compatible with Helios. To create a new project just run:

{% highlight bash %}
mvn org.apache.maven.plugins:maven-archetype-plugin:2.0-alpha-4:generate \
  -DarchetypeGroupId=org.beardedgeeks \
  -DarchetypeArtifactId=gae-eclipse-maven-archetype \
  -DarchetypeVersion=1.3.2 \
  -DarchetypeRepository=http://beardedgeeks.googlecode.com/svn/repository/releases
{% endhighlight %}

If you already have a project and want to upgrade to Helios, change the contents of the content tag in the MavenBuilder to:

{% highlight xml %}
<![CDATA[<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<launchConfiguration type="org.eclipse.ui.externaltools.ProgramBuilderLaunchConfigurationType">
<stringAttribute key="org.eclipse.debug.core.ATTR_REFRESH_SCOPE" value="$${project}"/>
<booleanAttribute key="org.eclipse.debug.ui.ATTR_LAUNCH_IN_BACKGROUND" value="false"/>
<stringAttribute key="org.eclipse.ui.externaltools.ATTR_LOCATION" value="${maven.home}/bin/${maven.executable}"/>
<stringAttribute key="org.eclipse.ui.externaltools.ATTR_RUN_BUILD_KINDS" value="full,incremental,auto,"/>
<stringAttribute key="org.eclipse.ui.externaltools.ATTR_TOOL_ARGUMENTS" value="war:exploded"/>
<booleanAttribute key="org.eclipse.ui.externaltools.ATTR_TRIGGERS_CONFIGURED" value="true"/>
<stringAttribute key="org.eclipse.ui.externaltools.ATTR_WORKING_DIRECTORY" value="${basedir}"/>
</launchConfiguration>]]>
{% endhighlight %}

And set up a the `maven.executable` property. By default it's `mvn` and I created a build profile for Windows environments that sets it to `mvn.bat`.

With the new 1.3 version the linked war folder is no longer required. Even with an existing project I'd suggest you to check out the new configuration from the example. If you're facing problems (or just want to praise me), go ahead and comment!

Update: Version 1.3.1 now has proper support for Windows!
