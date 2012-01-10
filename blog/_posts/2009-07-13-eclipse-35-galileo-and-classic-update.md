--- 
layout: post
title: Eclipse 3.5 Galileo and Classic Update
tags: 
- classic update
- Eclipse
- Eclipse 3.5
- Galileo
- plugins
- Programming
- software update
status: publish
type: post
published: true
author: hleinone
---

This is an update of my older [post](/blog/2009/01/20/eclipse-34-ganymede-and-classic-update). Some update sites like the Galileo Discovery Site or Eclipse Project Updates doen't seem to be compatible with the classic update due to them being solely P2 repositories. I keep getting: "Network connection problems encountered during search" when trying to get the plugin list from any those. Otherwise it seems to work well. Getting some of the Eclipse default plugins from other repositories was bit of a pain-in-the-ass, but after some manual labor the plugins started installing nicely.

Eclipse plugins are extremely cool, but the way they're being configured in Galileo (Eclipse 3.5) seems unsuitable for my needs and makes transferring the configuration to another environment difficult. The "new new" software update doesn't allow you to specify different install locations for different plugins, which is the case I'm after. Galileo - like Ganymede - also includes the "Classic Update", but in most of the installations it's not available. In Classic version you can enable it from **Window** -&gt; **Preferences** -&gt; **General** -&gt; **Capabilities**.

In reality, enabling that manually is quite simple, just browse to `WORKSPACE/.metadata/.plugins/org.eclipse.core.runtime/.settings/org.eclipse.ui.workbench.prefs` and add a new line containing:

    UIActivities.org.eclipse.equinox.p2.ui.sdk.classicUpdate=true

That's it, now you have both the new and classic updates available.

I use the classic update for installing plugins to a directory other than `eclipse/plugins`. I've been installing for example JDT to `eclipse/extensions/jdt` folder. The advantage from this is that when a new plugin breaks your whole installation, removing it is a lot easier. Also transferring the installation to another computer or a different Eclipse version should relatively easy.

Due to more and more plugin repositories supporting only P2, the classic update will become quite unusable in the future, so enjoy while you still can!
