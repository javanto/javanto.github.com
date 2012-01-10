--- 
layout: post
title: "Flex Builder and Maven - Simple SWC Project"
tags: 
- ActionScript 3
- Eclipse
- Flex
- Flex 3
- Flex Builder
- Flex Builder 3
- Flex-Mojos
- Maven
- Programming
- SWC
status: publish
type: post
published: true
author: hleinone
---

I've fallen in love with [Maven](http://maven.apache.org/), which is a powerful tool for project (in sense of the environment in the computer) handling.

I'm doing lots of Flex stuff and Maven - though designed for Java environment - works for that too. Using Maven in a Flex project requires a Maven plugin capable of compiling the AS3/Flex sources to a SWF or SWC. While Adobe has rejected the idea of releasing [official Maven support plugin](http://bugs.adobe.com/jira/browse/SDK-12730) they recommend using the most actively developed and probably the best one of the available alternatives, [Flex-Mojos](https://docs.sonatype.org/display/FLEXMOJOS/Home).

From this on I assume that you know the basics of the [Maven POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html). In this part I'll describe how to use Maven and Flex-Mojos to create a Flex Builder Flex library project. Since Flex-Mojos is under heavy development so we're using 3.1-SNAPSHOT version of it, so some of the mentioned things might be outdated or fixed soon (as of March 22th 2009 they aren't). In addition we're using Flex 3.3.0.

Since Maven projects are based on the POM, a Flex project requires some modifications there. First of all, we need to change the packaging to SWC:

{% highlight xml %}
<packaging>swc</packaging>
{% endhighlight %}

Apart from that we need the dependency to the Flex framework. That is found in the Sonatype public repository:

{% highlight xml %}
<repositories>
    <repository>
        <id>sonatype-flex</id>
        <url>http://repository.sonatype.org/service/local/repositories/flex/content</url>
    </repository>
</repositories>

<dependencies>
    <dependency>
        <groupId>com.adobe.flex.framework</groupId>
        <artifactId>flex-framework</artifactId>
        <version>3.3.0.4852</version>
        <type>pom</type>
    </dependency>
</dependencies>
{% endhighlight %}

The last thing we need is the Flex-Mojos plugin which does all the hard work. It's also found in the Sonatype repository:

{% highlight xml %}
    <pluginRepositories>
        <pluginRepository>
            <id>sonatype-flexmojos-snapshots</id>
            <url>http://repository.sonatype.org/service/local/repositories/flexmojos-snapshots/content</url>
        </pluginRepository>
    </pluginRepositories>

    <build>
        <sourceDirectory>src/main/flex</sourceDirectory>
        <outputDirectory>target</outputDirectory>
        <plugins>
            <plugin>
                <groupId>org.sonatype.flexmojos</groupId>
                <artifactId>flexmojos-maven-plugin</artifactId>
                <version>3.1-SNAPSHOT</version>
                <extensions>true</extensions>
                <configuration>
                    <includeClasses>
                        <class>com.googlecode.phlecks.example.Foo</class>
                    </includeClasses>
                    <includeFiles>
                        <param>foo.png</param>
                    </includeFiles>
                    <enableM2e>false</enableM2e>
                </configuration>
            </plugin>
        </plugins>
    </build>
{% endhighlight %}

Above we first specify the source folder which is src/main/flex instead of src/main/java. In includeClasses we specify the classes to be included in the library. Similarly we specify the other assets to be included in includeFiles. Also we specify that we're not using the m2eclipse Eclipse plugin, in case you use that, feel free to set this to true.

Now we've got a POM for our Flex library project, the next thing is to generate the Flex Builder project files with the plugin. So we type:
`mvn flexmojos:flexbuilder`
And as the result our project becomes a Flex library project also in Flex Builder after refreshing. If take a look at the project's properties and Flex library build path we see that in the classes tab src/main/flex is specified as the main source folder. In library path we notice that most of the Flex framework libraries are load from the local repository.

Also we notice that the current Flex-Mojos snapshot doesn't yet implement adding the includeClasses or includeFiles to the library, the output folder is still bin-debug and in the source path tab src/main/flex is also added as an additional source folder outside the main source folder.  These are smallish issues in the snapshot version, I've supplied the Flex-Mojos team a patch which fixes these.

So, now we've got a mavenized Flex library project that compiles in Flex Builder in the same way it would do if it was a Flex Builder created project. We can also compile it using Maven command:
`mvn package`
Which creates the SWC file, which is almost the same as the one created by Flex Builder. I think we're done by now, [here](http://code.google.com/p/phlecks/source/browse/#svn/flex-library-example/trunk)'s the link for a sample project configured this way.
