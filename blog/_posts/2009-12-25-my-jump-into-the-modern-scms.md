--- 
layout: post
title: My Jump into the Modern SCMs
tags: 
- CVS
- Eclipse
- Git
- Hg
- Mercurial
- Programming
- SCM
- Subversion
- SVN
- version control
status: publish
type: post
published: true
author: hleinone
---

I've been a somewhat faithful user of SVN since I started programming some 7 years back. It's better than CVS (which I've used for some time) or - I don't even know what kind of perverted - methods people used to use before that. It has worked relatively well in my very own projects and inadequately in the company space. I think the case is quite similar for many projects, the switch from CVS to SVN is now being followed with another switch.

In the last few years I've heard rumors about the next-generation SCMs like Mercurial, Git and Bazaar. I've heard huge open source projects moving out from SVN to one of those. As far as I understand, they're all based on similar approach: You clone the whole repository to your computer as a local repository, make your commits to the local repository and then push the changes back to the distributed one. Then the changes from the distributed repository are pulled to other people's local repositories and so should you do with the changes they have made.

For a while I've thought that the next-generation has already become current and I should really get back on track with the revision control. Few weeks ago a perfect moment appeared when I begun an open source pet project with a good friend of mine. We decided to use some modern technology for sharing the sources with each other. So we went on with Google Code which meant Mercurial.

First thing I want to know when introducing a new programming-related practice is its integration with the practices I already use. Read: I want IDE integration. And I also want to reserve the ability to change IDE and it still works. Of course I want to run it on multiple platforms, at least Windows and Linux. I want the ability to change from Eclipse + Windows to Netbeans + Linux or any combination of these, also as IntelliJ IDEA is being open sourced why not expect support for it too. I have to admit that that's a bit tough requirement for any system, but coming from Java world that's kind of a default expectation.

Mercurial works nicely out-of-the-box on Netbeans, probably because they use it a lot internally at Sun, for example on [OpenSolaris](http://hub.opensolaris.org/bin/view/Community+Group+tools/dcm_evaluation_mercurial). The Eclipse support is really impressive nowadays, the [HgEclipse](http://javaforge.com/project/HGE) is reaching the standards set by Subversive and Subclipse.

The things are completely the opposite on Git, [the Eclipse plugin](http://www.eclipse.org/egit/) is quite good - despite being still in incubation - and integrates well with the Team features. [The Netbeans integration](http://nbgit.org/) is not even able to push to the origin, which makes it mostly unusable.

As a conclusion, my expectations have after all been reached, and I expect to see bigger change in the near future, when the integration level raises on top of that of SVN which might be the reason restricting wider adoption in the enterprise. I don't know if there will be a single winner in the end. It seems that Mercurial and Git are quite even on features and adaption and Bazaar is not much behind. Mercurial is definitely ahead on IDE integration. If there will be a single winner, as a Finn, I have to hope [my countryman](http://en.wikipedia.org/wiki/Linus_Torvalds)'s product Git to be victorious. (For the serious readers, former is intended to be a joke, I'm really not patriotic at all, though Linus is a nice guy) I want to wish a merry Christmas to all my few readers!
