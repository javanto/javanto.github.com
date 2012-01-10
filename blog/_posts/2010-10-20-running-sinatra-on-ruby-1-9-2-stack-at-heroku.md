--- 
layout: post
title: Running Sinatra on Ruby 1.9.2 Stack at Heroku
tags: 
- Cloud
- cloud
- HashFresh
- Heroku
- Programming
- Ruby
- Ruby 1.9.2
- Sinatra
status: publish
type: post
published: true
author: hleinone
---
As my dear [Twitter](http://twitter.com/hleinone) followers may know, I've got a pet project called HashFresh (among others). Originally written in [Python for Google App Engine](http://code.google.com/appengine/docs/python/overview.html), I decided to port it to [Sinatra](http://www.sinatrarb.com/) and host it on [Heroku](http://heroku.com/). This post documents the challenges I faced as a Ruby newbie during the work I expected to be pretty trivial.

First of all, installing the latest Ruby version on Ubuntu. [Currently the latest release is 1.9.2](http://www.ruby-lang.org/en/news/2010/08/18/ruby-1-9-2-is-released/) and after reading from [Heroku documentation that 1.9.1 has a bug making Rails 3 unusable](http://docs.heroku.com/rails3), I decided - just to be safe - to go with the newest one. Boom! Lucid Lynx repositories' default Ruby version is 1.8.7 and Ruby 1.9.1 is provided as package named ruby1.9.1. Thankfully the Maverick Meerkat repository was already up and running and it contained Ruby 1.9.2 in the ruby1.9.1 package, oddly enough. Since I was not planning to go back to 1.8 and all the executables were named like ruby1.9.1, I created symbolic links from ruby, gem, rake etc. to ruby1.9.1, gem1.9.1, rake1.9.1 etc.

After that I had to install the gems my project required; sinatra, heroku, haml and maruku. That being done, I noticed I wasn't able to use them since they weren't on the $PATH, so up to editing `~/.profile` and adding to the end:

{% highlight bash %}
PATH="/var/lib/gems/1.9.1/bin:$PATH"
{% endhighlight %}

**Sidenote**: If you want to skip most of the former, use [RVM](http://rvm.beginrescueend.com/). It requires you to edit `~/.bashrc` if you're on Ubuntu as described on [the installation instructions](http://rvm.beginrescueend.com/rvm/install/), but I think it's definitely worth it. And it allows me to run multiple Ruby environments like [JRuby](http://www.jruby.org/), which is my next take.

All that Linux stuff now done, I could start the porting work which went all fine until I used [String#split](http://ruby-doc.org/core/classes/String.html#M000803). While working locally, it didn't work on Heroku. Running `heroku stack` showed me why, I was still using Ruby 1.8.7 as the production environment. But that shouldn't be a problem at all since Heroku supports also 1.9.2 stack.

But still, something has changed somewhere between the stacks. On 1.9.2 `"foo".split` results `["foo"]` in the HTML whereas on 1.8.7 it results `"foo"`. At least it is not the String API that has changed, because on both stack versions return `["foo"]` on `heroku console '"foo".split'`.

So, running `heroku stack:migrate bamboo-mri-1.9.2` was all I need to do for switching to 1.9.2, I thought. The truth is, it wasn't. The application crashed during startup and `heroku logs` didn't quite explain why. The error message was:

{% highlight irb %}
<internal:lib/rubygems/custom_require>:29:in `require': no such file to load -- hashfresh (LoadError)
        from <internal:lib/rubygems/custom_require>:29:in `require'
        from config.ru:1:in `block (3 levels) in <main>'
        ...
-----> Your application is requiring a file that it can't find.

       Most often this is due to missing gems, or it could be that you failed
       to commit the file to your repo.  See http://docs.heroku.com/gems for
       more information on managing gems.

       Examine the backtrace above this message to debug.
{% endhighlight %}

I did as it told me and examined the trace, but I couldn't figure out why it couldn't find the `hashfresh.rb` file, which clearly was in the root directory. `config.ru` stating `require 'hashfresh'`, as [instructed in the Heroku docs](http://blog.heroku.com/archives/2009/3/5/32_deploy_merb_sinatra_or_any_rack_app_to_heroku/). After a Heroku support request and a [Stack Overflow question](http://stackoverflow.com/questions/3973806/heroku-app-fails-to-start-require-no-such-file-to-load-sinatratestapp-lo), I found the answer. Between Ruby 1.9.1 and 1.9.2 they've changed the $LOAD_PATH not to include `.` directory automatically, so I had to state `require './hashfresh'` instead.

All's well that ends well. Programming with Sinatra on a simple project like this is very pleasing.
