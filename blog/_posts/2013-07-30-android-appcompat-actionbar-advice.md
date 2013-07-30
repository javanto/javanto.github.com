--- 
layout: post
title: Android AppCompat ActionBar Advice
tags: 
- AppCompat
- ActionBar
- action bar
- Android
- Gradle
- support library
- Holo
- Jelly Bean
- Google
status: publish
type: post
published: true
author: hleinone
---

Couple of weeks ago, along with the Android 4.3 Jelly Bean, Google released a new [AppCompat support library](http://developer.android.com/tools/support-library/index.html) bringing the [action bar](http://developer.android.com/guide/topics/ui/actionbar.html) all the way down to devices running Android 2.1. I couldn't find a simple example on the web combining Gradle and the new library, so I decided to do it myself.

First off, I created a new Android project, using the new [Gradle project structure](http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Project-Structure) and a simple Gradle build file. In the build file, apart from the basic stuff you can find [all over the place](http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Simple-build-files), you need to add a dependency to the AppCompat library.

{% highlight groovy %}
dependencies {
  compile "com.android.support:appcompat-v7:18.0.+"
}
{% endhighlight %}

You still need to have your activity to extend [ActionBarActivity](http://developer.android.com/tools/support-library/index.html) and set up the theme in your manifest to be one of `@style/Theme.AppCompat`, `@style/Theme.AppCompat.Light` or `@style/Theme.AppCompat.Light.DarkActionBar`, of course it's also fine to use a theme of your own that derives from one of those. There you have it, the simplest possible action bar supporting 2.1 and up.

![Action bar on Android 2.1](../../../../../img/2013/07/30/android-appcompat-actionbar-advice/action-bar-on-android-2.1.png)

I wanted to go one step further and make it more visually appealing. The app I use as an example was made as a joke for "tracking" the birth of [Prince George of Cambridge](https://en.wikipedia.org/wiki/Prince_George_of_Cambridge). The [Royal Baby Tracker](https://play.google.com/store/apps/details?id=com.javanto.rbt) was made during my flight to London and took far less time than Catherines labor. After his royal prince-ness was born, I made the changes described here and released the app as [open source](https://github.com/javanto/royal-baby-tracker).

So, I created a theme of my own that derived from `@style/Theme.AppCompat`. The only thing I really wanted to customize was the background color, for which I had to set up the following in `values/styles.xml`:

{% highlight xml %}
<style name="Theme.Base.Rbt" parent="@style/Theme.AppCompat">
  <item name="actionBarStyle">@style/Widget.Rbt.ActionBar</item>
  <item name="windowActionBar">true</item>
</style>

<style name="Widget.Rbt.ActionBar" parent="@style/Widget.AppCompat.ActionBar">
  <item name="background">@color/royal_blue</item>
</style>
{% endhighlight %}

That declares the default style ie. style for in pre-[Holo](http://developer.android.com/design/style/themes.html) Android versions. In `values-v14/styles.xml` I'm then having the following:

{% highlight xml %}
<resources xmlns:android="http://schemas.android.com/apk/res/android">
<style name="Theme.Base.Rbt" parent="@style/Theme.AppCompat">
  <item name="android:actionBarStyle">@style/Widget.Rbt.ActionBar</item>
  <item name="android:windowActionBar">true</item>
</style>

<style name="Widget.Rbt.ActionBar" parent="@android:style/Widget.Holo.ActionBar">
  <item name="android:background">@color/royal_blue</item>
</style>
</resources>
{% endhighlight %}

The differences between these two files are minimal, but important. In both the `Theme.Base.Rbt` derives from the very same AppCompat theme. In the default style I can't use the `android:` namespace since those attributes were only introduced in Holo. But the AppCompat library has the respective attributes in the default namespace. In the v14 version the `Widget.Rbt.ActionBar` derives from the Holo version of the widget, which is completely fine as we know that since Ice Cream Sandwich the Holo is there.

And the end result? I'm just waiting for my knightening!

![Styled action bar](../../../../../img/2013/07/30/android-appcompat-actionbar-advice/styled-action-bar.png)

If you found this article useful and for that, or some other reason, want to support me, go and buy the [Royal Baby Tracker app on Google Play Store](https://play.google.com/store/apps/details?id=com.javanto.rbt).