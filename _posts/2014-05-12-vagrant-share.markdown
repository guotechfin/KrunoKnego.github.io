---
layout: post
title:  "Vagrant share!"
date:   2014-05-12 23:06:00
categories: vagrant tools
tags: [vagrant]
---

![Vagrant]({{ site.url }}/assets/vagrant.jpg ){: .center-image }

If you're doing web development and you never heard of Vagrant you better catch up and see what's the
fuss all about. Basically once you see what Vagrant can do for you, you'll wonder why such tool
didn't come sooner. To read more about Vagrant you can do so at
<a title="Vagrant up" href="http://www.vagrantup.com" target="_blank">Vagrant</a>

This blog post will talk about a little more advanced feature of Vagrant that came up in the 1.5 release.
It's called Vagrant Share.

Now before I explain to you everything about vagrant share imagine a situation where you're developing
for a client but didn't set up a server and you can't show him your progress because he's on a remote
location. That's where vagrant share kicks in.
With it you can show your clients your project without setting up and pushing your files to a server.

Now how to enable such a thing? Well it's fairly simple.

First go to <a title="Vagrant cloud" href="https://vagrantcloud.com/" target="_blank">Vagrant Cloud</a> and create an
account.

Afterwards go ahead and open your terminal and enter

{% highlight bash  %}
    vagrant login
{% endhighlight %}

Use same username/password combination you used at vagrant cloud.

Finally just enter

{% highlight bash  %}
    vagrant share
{% endhighlight %}

Now share the link you got with your client and that's it.
In case vagrant share didn't automatically recognize correct port you can configure it manually.
In my case it used port 8000 and I wanted it to go through port 80. So you just use the following
command

{% highlight bash  %}
    vagrant share --http 80
{% endhighlight %}

There's also another feature of vagrant share called vagrant connect.
Vagrant connect works only if both you and your client have vagrant installed.
The only difference is that you don't need to give to your client complete url. You just give to him
your machine name that was generated using the vagrant share command and he connects to it using this
command


{% highlight bash  %}
    vagrant connect your_machine_name
{% endhighlight %}


This feature will make your life easier if you're constantly dealing with clients that want you to show them
your progress on their websites or if you just want to show off to your friends.
