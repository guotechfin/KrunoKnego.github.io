---
layout: post
title: "PHPStorm and Symfony2"
category: 
date: " 2014-08-14 11:09:39"
tags: []
---

![PHPStorm]({{ site.url }}/assets/phpstorm.gif "PHPStorm Image"){: .center-image}

PHPStorm like its name suggests is IDE for PHP. It's very advanced IDE with lots of
features like multi-line editing, search&replace with regular expressions etc.

PHPStorm's code is built with JAVA and it has really lots of plugins pre-installed so if
you have a little slower computer I suggest you un-check a few.

This article assumes that you're relatively familiar with Symfony2 and since you're familiar I'll
assume you also know about Symfony2 services.

Now I don't know about you but I love using Symfony2 service and I like the whole idea behind
service oriented architecture.

The problem is that if you wish to use services in your controller you have to memorize service names.
To solve that problem you can use PHPStorm's plugin called <a title="PHPStorm Plugin Symfony2" href="http://plugins.jetbrains.com/plugin/7219?pr=phpStorm" target="_blank">"Symfony2 plugin"</a>.

It reads the cache of your symfony2 application and suggests you services as you type them in.

![symfony2,cache]({{ site.url }}/assets/phpstorm1.jpg "Symfony2 cache")

Apart from enabling you to access your services it enables you to create them relatively easy.


For example, if you were to create a service

{% highlight php5 %}
    <?php

    namespace SomeService;

    use SomeRepo;
    use Symfony\Component\HttpFoundation\RequestStack;

    class SomeService
    {
        private $someRepo;
        private $requestStack;

        public __construct(SomeRepo $someRepo, RequestStack $requestStack)
        {
            $this->someRepo = $someRepo;
            $this->requestStack = $requestStack;
        }

        ...
    }
{% endhighlight %}

Afterwards you right click on your service and click create service. Notice how I declared classes in constructor
that will enable symfony2 plugin to automatically detect services that are to be injected for you.

![symfony2,create_service]({{ site.url }}/assets/phpstorm2.jpg "Create service image")

This is only small part of what PHPStorm can do for you so if you're doing any kind of PHP development I
suggest you check  <a title="Jetbrains PHPStorm" href="http://confluence.jetbrains.com/display/PhpStorm/PhpStorm+Early+Access+Program" target="_blank">PHPStorm 8 EAP</a> ( Early Access Program )




