--- 
layout: post
title: Creating A Simple Maven Plugin
tags: 
- example
- Java
- Maven
- merge
- merging
- Mojo
- plug-in
- plugin
- Programming
- properties
- tutorial
status: publish
type: post
published: true
author: hleinone
---
I've been a big admirer of [Maven](http://maven.apache.org) for a long time. For some reason I have never made a plugin of my own. Today I did it, and I have to say it was a great learning experience. I will introduce all the neat things I figured out during the development. So let the story begin.

At work there was some need for a plugin that could help us with merging the propreties files. After trying to find a ready made solution it was quite clear that doing it in-house would be a very low priority since the result is already accomplished using Ant. But as the task being quite interesting, straightforward and nicely small, in addition to the fact of me being on a sick leave, I decided to do it on my own and release it under Apache v2 license. The requirement for the plugin was to have environment/user-specific properties files and merge them with some default ones, so that the more specific file's properties override the ones from the default. The resulting properties files are then mainly used in [Spring](http://www.springsource.org) configurations. The fact of this being implemented with Ant has been the main issue preventing us from switching completely to Maven.

While getting my hands dirty with the code I noticed that all Maven plugins are basically MOJOs (Maven plain Old Java Object). So it was a MOJO that I was after! I created one which I gave the name MergePropertiesMojo. Like all the other MOJOs, it extends the AbstractMojo class which gives access to some project attributes, but for this case I didn't need any. I was just required to implement the `execute()` method.

In addition to the plain coding I had to figure out how the plugin would be used. After employing the little gray brain cells for a while my conclusion was that: first-of-all the user must configure the plugin by setting up the properties files to be merged, which consist of a target file and a bunch of files to merge. A good example tells more than thousand words:

{% highlight xml %}
<configuration>
	<merges>
		<merge>
			<targetFile>${build.outputDirectory}/application.properties</targetFile>
			<propertiesFiles>
				<propertiesFile>src/main/config/${user.name}/application.properties</propertiesFile>
				<propertiesFile>src/main/config/default/application.properties</propertiesFile>
			</propertiesFiles>
		</merge>
	</merges>
</configuration>
{% endhighlight %}

This is the plugin configutation. In this case we have a default properties file which we want to merge with the user's own properties file. In some other case we could replace ${user.name} with something else. Note that the order of the propertiesFile elements matters since the properties from the file above override the ones from the files below it on the listing. As a result we get an application.properties file with the merged properties to appear in the output directory. As you might have noticed it uses src/main/config as the directory for storing the different properties files. It isn't that clear (at least for me) what's the directory for but it suits for this case quite well.

In Maven plugins the user can set up the execution phase in which the plugin executes. So that, if we want the merging to occur on every time the project is compiled we can do it. Here's a proof:

{% highlight xml %}
<executions>
	<execution>
		<phase>compile</phase>
		<goals>
			<goal>merge</goal>
		</goals>
	</execution>
</executions>
{% endhighlight %}

This makes the merge occur automatically on each compilation. Different executions can also have different configurations, just by putting the configuration element under the execution. We could then merge some files on compilation and others on package for example. The whole execution thing is not mandatory, since the plugin can also be run manually using mvn merge-properties:merge, but it usually makes things easier as learning new goals and remembering to run them is not required.

Back to the configuration. The merges and merge elements specify that in my MOJO, there's an array called merges which in my case is of type Merge. Merge is a simple POJO (Plain Old Java Object) that contains the fields targetFile of type File and an array of Files called propertiesFiles. There are some special [Maven Javadoc annotations](http://maven.apache.org/developers/mojo-api-specification.html) for both the class and the fields that are required for instance in injection of the configuration values to corresponding fields.

On the class I used `@goal merge` which specifies the name of the plugin goal to start execution (merge in this case) and `@requiresProject` which specifies that the plugin is required to be run inside of a project. For the fields I used `@description` and `@required` which I think are quite self descriptive and `@parameter` which specifies the field being Maven injected. The `@parameter` also allows us to set a default value or command-line parameter to use instead of configuration, but I didn't use those. More details on these can be found on the [Maven MOJO API](http://maven.apache.org/developers/mojo-api-specification.html).

After setting up these, the whole thing was all about implementing the execute() method. Since it's relatively trivial Java code, it's not relevant to go through it here. As code without a test doesn't exist, I made unit test for my MOJO. Using [EasyMock](http://easymock.org) and its class extension in addition to [PowerMock](http://code.google.com/p/powermock) I was able to test the functionality. Also let's not go into further detail with that since it's completely off-topic and the Internet is full of great tutorials regarding that. You can always take a look at my implementation details on [SVN](http://code.google.com/p/beardedgeeks/source/browse/#svn/maven-merge-properties-plugin/trunk).

Basically the job was now done, all there was left was the project administration. I chose [Google Code](http://code.google.com) as the project hosting platform and SVN as the version control system. I've setup these to the plugin's POM. The nicest thing I found out was that I can use the SVN repository as a Maven repository and even deploy to there using the deploy plugin. How to do that is described in [here](https://wagon-svn.dev.java.net). Now I'm off to making the first release of my plugin. I hope you find the plugin useful, or even better this article useful!

[The home of maven-merge-properties-plugin at Google Code](http://code.google.com/p/beardedgeeks).
