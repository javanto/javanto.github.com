--- 
layout: post
title: Eclipse 3.4 Ganymede and Classic Update
tags: 
- classic update
- Eclipse
- eclipse 3.4
- ganymede
- plugins
- Programming
- software update
status: publish
type: post
published: true
author: hleinone
---

Eclipse plugins are extremely cool, but the way they're being configured in Ganymede (Eclipse 3.4) is awful and makes transferring your configuration to another environment almost impossible. The new software update doesn't allow you to specify different install locations for the plugins. Ganymede also includes the "Classic Update", but in most of the installations it's not available. Some have reported that in the [Eclipse Classic](http://www.eclipse.org/downloads/packages/) it can be set through **Window** -&gt; **Preferences** -&gt; **General** -&gt; **Capabilities**. If that is not your case, please follow, cause I'll explain to you how to enable the classic update.

In reality that is quite simple, just browse to `WORKSPACE/.metadata/.plugins/org.eclipse.core.runtime/.settings/org.eclipse.ui.workbench.prefs` and add a new line containing:

    UIActivities.org.eclipse.equinox.p2.ui.sdk.classicUpdate=true

That's it, now you have both the new and classic updates available.

I use the classic update for installing plugins to a directory other than `eclipse/plugins`. I've been installing for example JDT to `eclipse/3rdp/jdt` folder. The advantage from this is that when a new plugin breaks your whole installation, removing it is a bit easier. Also transferring the installation to another computer or a different Eclipse version should be easier.

Hope you find this useful!
