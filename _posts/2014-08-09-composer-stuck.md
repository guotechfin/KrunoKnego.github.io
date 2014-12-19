---
layout: post
title: "Composer stuck?"
category: 
date: " 2014-08-09 12:19:14"
tags: []
---

![Composer]({{ site.url }}/assets/composer.jpg "Composer Image"){: .center-image}

I use composer for my daily development needs and I love it.
Usually everything is okay but when I worked on a Sylius project that has lots
of dependencies sometimes it got stuck when I tried to do

{% highlight bash  %}
    composer update
{% endhighlight %}

A few days later my colleague stumbled upon a <a title="Composer Issue" href="https://github.com/composer/composer/issues/1898" target="_blank">github issue</a>
that basically says composer has a problem with memory.

So I bumped my Vagranfile from default 384 MB to 1024MB and later on when I used composer update
command it took a while but it completed it successfully.